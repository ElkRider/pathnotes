#### 1. log file 



| 日志类型                                | 文件位置 (以 `datadir` 为例)                                 | 文件名/格式                                                  | 查找方法                                                     |
| :-------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| **Redo Log** (重做日志, InnoDB引擎特有) | `/var/lib/mysql/` 或 配置中的 `innodb_log_group_home_dir`    | MySQL 8.0 前: `ib_logfile0`, `ib_logfile1` ... MySQL 8.0 起: `#innodb_redo` 目录下的 `#ib_redoXXX` 文件 | `SHOW VARIABLES LIKE 'innodb_log_%';` `SHOW VARIABLES LIKE 'datadir';` |
| **Undo Log** (回滚日志, InnoDB引擎特有) | **MySQL 5.6/5.7 及更早**: `datadir` 目录下的共享表空间文件 `ibdataN`  **MySQL 8.0 及更新**: `datadir` 目录或由 `innodb_undo_directory` 指定 | **MySQL 8.0 起**: `undo_001`, `undo_002` ...                 | `SHOW VARIABLES LIKE 'innodb_undo_%';` `SELECT TABLESPACE_NAME, FILE_NAME FROM INFORMATION_SCHEMA.FILES WHERE FILE_TYPE LIKE 'UNDO LOG';` |
| **Binlog** (二进制日志, Server层日志)   | `datadir` 目录或由 `log_bin_basename` 配置项指定             | 基础名 + 数字索引后缀，例如 `mysql-bin.000001`, `mysql-bin.000002` ... | `SHOW BINARY LOGS;` `SHOW MASTER STATUS;`                    |

 

##### open bin_log 

```shell
-- SHOW BINARY LOGS;
SHOW VARIABLES LIKE 'log_bin';


my.ini
## ygx add bin-log open 
#log_bin = C:\ProgramData\MySQL\MySQL Server 5.7\Data\mysql-bin.log   # 路径和文件名前缀
#server_id = 1                            # 必须设置，集群中唯一
#binlog_format = ROW                      # 推荐 ROW 格式

restart mysql server
```







#### 2.revoke privilege of database manager

todo:



##### 3. how to stop transaction handling safely

#### 

#### 4.how to migrate mysql to other kind database



#### 5.how to use stored procedure



#### 6.how to migrate mysql to other local mysql db



#### 7.how to use recurive sql on mysql8

compare with application lanage , database implement is some different by keyword WITH ,mysql 5 not support this recursive way .

```sql
-- 方法1：为所有机构找到对应的省级机构
-- 思路：所有机构（除了根机构）都需要映射到省级机构
-- 1. 省级机构（level=2）映射到自己
-- 2. 其他级别机构通过递归向上找到省级机构

WITH RECURSIVE all_orgs AS (
    -- 第一步：获取所有需要映射的机构（排除根机构）
    SELECT 
        id,
        org_code,
        org_name,
        parent_id,
        level,
        id as original_id,
        -- 如果是省级机构，标记为已找到省级机构
        CASE WHEN level = 2 THEN id ELSE NULL END as prov_id,
        0 as depth
    FROM coop_org_main_info 
    WHERE del_flag = '0' 
        AND org_code NOT IN ('00000', '00001', '00002')
    
    UNION ALL
    
    -- 第二步：递归向上查找省级机构
    SELECT 
        ao.id,
        ao.org_code,
        ao.org_name,
        p.parent_id,
        p.level,
        ao.original_id,
        -- 如果找到省级机构，记录其ID
        CASE 
            WHEN p.level = 2 THEN p.id
            ELSE ao.prov_id
        END as prov_id,
        ao.depth + 1
    FROM all_orgs ao
    JOIN coop_org_main_info p 
        ON ao.parent_id = p.id 
        AND p.del_flag = '0'
    WHERE ao.prov_id IS NULL  -- 还没找到省级机构时继续递归
        AND ao.depth < 10  -- 防止无限递归
)
-- 获取最终结果
SELECT DISTINCT
    ao.id,
    ao.org_code,
    ao.org_name,
    ao.level as org_level,
    p.org_code as prov_org_code,
    p.org_name as prov_org_name,
    p.level as prov_level
FROM all_orgs ao
LEFT JOIN coop_org_main_info p 
    ON ao.prov_id = p.id
WHERE ao.prov_id IS NOT NULL
    -- 确保每个机构只取最终结果
    AND ao.id = ao.original_id
ORDER BY ao.org_code;
```


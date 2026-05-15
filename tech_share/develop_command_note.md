# 开发过程常用命令记录

## 一、linux运维常见的命令

### 1、系统常用命令

#### 1.1 linux 快捷链接

ln -s  /mnt/bigdisk/dev_workspace/dev_running_data/1panel/  1panel

```shell
root@lckfb:~# ll

total 172

dr-xr-x--- 11 root root  4096 May 11 23:19 ./

drwxr-xr-x 25 root root  4096 Jan 15 23:03 ../

lrwxrwxrwx  1 root root    51 May 11 12:02 1panel -> /mnt/bigdisk/dev_workspace/dev_running_data/1panel/
```



#### 1.2、系统注册服务查看

```shell
systemctl status
systemctl start docker
systemctl restart docker
systemctl stop docker
```







#### 1.3、常见的终端工具

```shell

[screen] 终端复用功能最强
查看有哪些会话在跑	          screen -ls
重新连进去（只有一个会话时）	  screen -r
重新连进去（有多个会话时）	   screen -r mytask
强制连（会话状态异常时）	    screen -d -r mytask
结束会话	                  进去后输入 exit 或按 Ctrl+D


[tumx] 更加现代化的工具
查看所有会话	           tmux ls
重新连进去	            tmux attach -t mytask
结束会话	             tmux kill-session -t mytask
创建会话的同时给它起名	   tmux new -s mytask

[nohup] 上古神器
nohup 你的命令 &
# 举例：运行 Python 脚本，并把输出存到 mylog.log
nohup python train.py > mylog.log 2>&1 &

---
&	放到后台运行
> 文件名	标准输出（正常日志）写到这个文件
2>&1	错误信息也写到同一个文件（不然可能悄无声息地出错你都不知道）
nohup	忽略挂断信号，关终端不会杀它
---


```

```shell

│ 关掉终端后程序继续跑  
│  【screen】                                                 │
│  screen -S 名字       # 创建                                │
│  Ctrl+A 然后 D        # 分离（关键！）                      │
│  screen -r 名字       # 重连                                │
│  screen -ls           # 查看所有                            │
│                                                             │
│  【tmux】                                                   │
│  tmux new -s 名字     # 创建                                │
│  Ctrl+B 然后 D        # 分离                                │
│  tmux attach -t 名字  # 重连                                │
│  tmux ls              # 查看                                │
│                                                             │
│  【nohup】（简单任务）                                       │
│  nohup 命令 > 日志 2>&1 &                                   │
│  tail -f 日志         # 查看输出                            │
└─────────────────────────────────────────────────────────────┘
```

#### 1.4、系统命令手册查看

```shell
man
help
```

#### 1.5、常用的操作命令

```shell
tar 
zip
rpm
lsblk
free
top
ssh
watch
tail
cat
less
vim
nano
fdisk
mount
ln

`find` 是一个非常强大的命令行工具，可以用来搜索文件和目录。以下是一些使用 `find` 查找大文件的例子：

1. **查找某个目录下大于指定大小的文件**：
   
   例如，要在 `/var/log` 目录下查找大于 100MB 的文件，可以使用如下命令：
   ```bash
   find /var/log -type f -size +100M
   ```
   这里的 `+100M` 表示查找大小超过 100MB 的文件。你可以根据需要调整这个值。

2. **查找并按大小排序**：

   如果您想要找到目录下的所有文件并按大小排序，可以结合 `ls` 和 `sort` 命令使用：
   ```bash
   find /path/to/search -type f -exec ls -lhS {} +
   ```
   这个命令会列出指定路径下所有的文件，并按照文件大小从大到小排序。

```



#### 1.6、配合调试其他系统的命令

```shell
adb
telnet
curl
ping 
tracert
```

#### 1.7、linux的文件系统常见的命令

~~~shell
df 命令用于显示磁盘分区的使用情况，包括总容量、已用空间、可用空间和挂载点
df -h
-h 选项表示以人类可读的格式（如 MB 或 GB）显示磁盘使用情况。



du 命令用于显示目录或文件的磁盘使用情况。
du -sh /path/to/directory
- -s 选项表示汇总总计。
- -h 选项表示以人类可读的格式显示。



lsblk 命令用于列出所有块设备的信息，包括磁盘和分区。



``` shell
NAME            MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda               8:0    0   50G  0 disk 
├─sda1            8:1    0  500M  0 part /boot
└─sda2            8:2    0 49.5G  0 part 
  ├─centos-root 253:0    0   50G  0 lvm  /
  └─centos-swap 253:1    0    2G  0 lvm  [SWAP]
```

fdisk  命令用于查看和管理磁盘分区。你可以使用 `-l` 选项来列出所有磁盘和分区的信息。


fdisk -l


```
Disk /dev/sda: 50 GiB, 53687091200 bytes, 104857600 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x12345678

Device     Boot  Start       End   Sectors  Size Id Type
/dev/sda1  *      2048    1026047   1024000  500M 83 Linux
/dev/sda2       1026048 104857599 103831552 49.5G 8e Linux LVM
```
~~~



#### 1.8、vim常见使用

```shell
:%s\要替换的文本\替换后的文本
注意这里的\在处理复杂文本可能会出现冲突
```







## 二、docker

#### 2.1、show doker active server status

```bash
[root@192 ~]# docker system df
TYPE                TOTAL               ACTIVE              SIZE                RECLAIMABLE
Images              13                  5                   10.67 GB            8.544 GB (80%)
Containers          10                  2                   131.6 kB            131.6 kB (99%)
Local Volumes       2                   0                   132 kB              132 kB (100%)

查看特定容器的资源使用情况
docker stats <container_id_or_name>


查看容器的进程
使用 docker top 命令可以查看容器内的进程：

查看容器的事件
使用 docker events 命令可以实时查看 Docker 守护进程的事件：
```

#### 2.2、export  all docker images

```bash
bash
复制代码运行
docker save -o all_images.tar $(docker images -q)
   
这个命令会将所有镜像打包成一个名为all_images.tar的文件。其中，docker images -q用于获取所有镜像的ID，然后通过docker save命令将这些镜像保存到指定的文件中。
```

#### 2.3、docker compose 的升级

```shell
curl -SL https://github.com/docker/compose/releases/download/v2.29.4/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose

chmod +x /usr/local/bin/docker-compose
docker-compose --version
```

#### 2.4、查看容器的日志

```shell
使用 docker logs 命令可以查看容器的日志输出：

查看容器的实时日志
使用 docker logs -f 命令可以实时查看容器的日志输出：
```

#### 2.5、查看 `my_container` 的详细信息：

~~~shell
```sh
docker inspect my_container
```
~~~

#### 2.6、清理现有 Docker 网络

~~~shell
如果存在多个 Docker 网络或者有损坏的网络，它们可能会导致冲突。你可以列出所有的 Docker 网络并移除不必要的网络。
```bash
docker network ls
docker network rm <network_id_or_name>
```
~~~

#### 2.7、docker设置内存运行

~~~shell
示例：为 RocketMQ 指定 200M 内存

```bash
docker run -d \
  --name rocketmq-broker \
  --memory="200m" \
  rocketmqinc/rocketmq:latest
```


```bash
docker run -d \
  --name nacos-server \
  --memory="512m" \
  nacos/nacos-server:latest
```

除了硬性的内存限制外，还可以设置一个软性限制 (`--memory-reservation`)，它指定了当系统内存不足时优先分配给该容器的最小内存量。这有助于在多租户环境中更好地管理资源竞争。

```bash
docker run -d \
  --name my-app \
  --memory="256m" \
  --memory-reservation="128m" \
  my-app-image:tag
```


配置交换分区 (Swap)

你还可以控制容器是否可以使用 swap 分区，以及如果使用的话，swap 的大小相对于物理内存的比例。例如，禁止使用 swap 或者设置一个合理的比例：

```bash
docker run -d \
  --name my-app \
  --memory="256m" \
  --memory-swap="512m" \  # 允许最多 256M 物理内存 + 256M swap
  my-app-image:tag
```

4. 使用 Docker Compose

如果你使用 Docker Compose 来管理多个服务，可以在 `docker-compose.yml` 文件中定义每个服务的资源限制。

```yaml
version: '3'
services:
  rocketmq-broker:
    image: rocketmqinc/rocketmq:latest
    container_name: rocketmq-broker
    deploy:
      resources:
        limits:
          memory: 200M
    # ...其他配置项...
  
  nacos-server:
    image: nacos/nacos-server:latest
    container_name: nacos-server
    deploy:
      resources:
        limits:
          memory: 512M
   -- # ...其他配置项...
    
```
然后使用 `docker-compose up -d` 启动这些服务。
~~~


##### 1、gitignore configration

```
🔗 官方参考目录
Git 官方文档：Git - gitignore Documentation

GitHub 官方模板：github/gitignore





```

```shell
git rm -r --cached .idea   # 从Git中移除.idea目录，但保留本地文件
git add .gitignore         # 添加新的忽略规则
git commit -m "chore: 忽略.idea文件夹"  # 提交更改
```

```
常见配置：
.idea/ (推荐)：最简洁高效。匹配项目内任意层级名为 .idea 的目录及其所有内容。团队协作时最方便。


```


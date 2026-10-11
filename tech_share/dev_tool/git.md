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

#### 2. git  look remote all branch 

```shell
在合并代码的时候用了一个比较笨拙的工具(soureTree)，这里已存在的是最原始的给远程一个待合并副本，本地另一个分支一个副本，当前分支一个副本。而且页面可读性非常差。导致手动合并速度非常麻烦，但是竟然直接给合并了，现在想弄个干净的重新弄下

git 删除master分支，并从远程服务器拉取最新的 master分支。


1. 确保当前不在 master 分支上
如果你正处于 master 分支，需要先切换到其他分支（例如 main 或任意临时分支）：

bash
git checkout main   # 或者 git checkout -b temp
2. 更新远程仓库信息
bash
git fetch origin
3. 删除本地的 master 分支
bash
git branch -D master
-D 是强制删除，因为本地 master 可能包含未合并的提交（正是你想丢弃的那些）。

4. 从远程重新创建 master 分支
bash
git checkout -b master origin/master
这条命令会基于 origin/master 创建一个新的本地 master 分支并自动切换过去。

5. （可选）确认状态
bash
git log --oneline -5   # 查看最近几次提交，应该和 origin/master 完全一致

```


















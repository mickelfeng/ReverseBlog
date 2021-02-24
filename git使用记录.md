---
title: git使用记录
date: 2021-02-22 20:30:08
tags: git
category: 其他
---

本文主要记录 git 使用过程中需要搜索的一些命令。

### 如何恢复初始的 git 提交。
您可以删除 HEAD 并将存储库还原到新的状态，在该状态下可以创建一个新的初始提交：
```bash
git update-ref -d HEAD
```

创建新提交之后，如果您已经将其推入远程，则需要强制将其发送到远程，以便覆盖先前的初始提交：
```bash
git push --force origin
```
> 不要使用 `rm -rf .git` 或者像这样的任何操作，这样都会彻底清除整个存储库，包括所有其他分支，以及您试图重置的分支。

### 撤销 git add和 commit 操作
还没有 push 的时候使用 reset 命令。
```bash
git reset --mixed commit_id    #不删除工作空间改动代码，撤销 commit ，并且撤销 git add . 操作，默认操作。
git reset --soft  commit_id    # 不删除工作空间改动代码，撤销 commit ，不撤销 git add .  。
git reset --hard commit_id     # 删除工作空间改动代码，撤销 commit ，撤销 git add . 慎用这个命令。
git reset –hard origin/master  # 将本地的状态回退到和远程的一样
git commit --amend             # 只是改一下注释
```

已经 push 了，可以使用 git revert 还原已经提交的修改 ，此次操作之前和之后的 commit 和 history 都会保留，并且把这次撤销作为一次最新的提交。
```bash
git revert HEAD          # 撤销前一次 commit 。
git revert HEAD~n        # 撤销前n次 commit 。
git revert commit-id     # 撤销指定的版本，撤销也会作为一次提交进行保存。
git reset HEAD^ file     # 回退 flie 这个文件的版本到上一个版本
```
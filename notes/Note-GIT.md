---
layout: page
title: GIT
category: note
tags: Others
---

* content
{:toc}

# Git

``` shell
# Initialize
git init

# check status
git status

# ckeck difference
git diff

# 查看版本
git log

# 查看commit历史
git reflog

# 回滚
git reset --hard HEAD^ # 回滚到上个版本
git reset --hard HEAD@{5} # 回滚到上5个版本

# 将工作区文件回到暂存区或版本库版本
git checkout -- file

# 讲暂存区文件回到版本库版本
git reset --hard HEAD # 回到最后一次commit版本

# 删除文件
rm test.txt
git rm
git commit -m "remove test.txt"

# 恢复删除
git checkout -- test.txt

# 创建分支
git branch dev

# 切换分支
git checkout dev

#查看分支
git branch

# 合并分支
git merge dev
git merge --no-ff -m "merge with no-ff" dev

# 删除分支
git branch -d dev
git branch -D dev # 删除还没合并的分支

# 查看远程仓库
git remote
git remote -v

# 推送到远程仓库
git push origin master
git push origin dev

# 在本地创建和远程分支对应的分支
git checkout -b branch-name origin/branch-name

# 建立本地分支和远程分支的关联
git branch --set-upstream branch-name origin/branch-name

# 抓取远程的新提交
git pull

# 将分叉整理成直线
git rebase
```


# Github

``` shell
# clone from github
git clone https://github.com/cyzlmh/cyzlmh.github.io

# Submit changes
git add --all
git commit -m "some comments"
git push -u origin master
```

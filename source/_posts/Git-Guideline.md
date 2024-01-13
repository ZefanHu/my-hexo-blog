---
title: Git Guideline
date: 2024-01-13 23:27:45
tags:
    - "Git"
thumbnail: https://cdn.jsdelivr.net/gh/ZefanHu/blogImage@main/img/20240113232846.png
---

**最近学习了如何使用hexo搭建一个属于自己的博客，我在部署的过程中遇到了一些问题，在此记录。**

## Cloudflare Page



**cloudfalre page部署hexo博客使用网络教程中的hexo generate会报错**：**hexo: not found**

**正确的配置写法是：**

* **生产分支** main

* **构建命令** npm run build

* **构建目录** public

## Git Guideline

**Git 是一个强大的版本控制系统，它允许多人协作开发项目，同时跟踪和管理代码的历史变更。以下是一些在学习Git过程中最重要的命令和概念。**

### 初始化 Git 仓库

- **git init**: 在当前目录下初始化一个新的Git仓库。

### 添加远程仓库

- **git remote add origin [仓库地址]**: 将本地仓库与远程仓库关联起来。`origin`是远程仓库的默认别名。

### 查看远程仓库

- **git remote -v**: 查看当前配置的远程仓库地址。

### 删除远程仓库

- **git remote remove [别名]**: 删除与远程仓库的关联。

### 添加文件到暂存区

- **git add [文件名]**: 将指定文件添加到暂存区。
- **git add .**: 将所有更改的文件添加到暂存区。

### 提交更改

- **git commit -m "[提交信息]"**: 将暂存区的内容提交到本地仓库。

### 分支管理

- **git branch**: 列出所有本地分支。
- **git checkout [分支名]**: 切换到指定分支。
- **git checkout -b [新分支名]**: 创建并切换到新分支。

### 推送到远程仓库

- **git push origin [分支名]**: 将本地分支的更改推送到远程仓库。
- **git push -u origin [分支名]**: 推送本地分支到远程仓库，并设置上游（跟踪）关系。

### 拉取远程仓库的更改

- **git pull origin [分支名]**: 从远程仓库拉取更改并合并到当前分支。

### 本地分支与远程仓库分支的注意事项

- 确保本地分支名称与远程仓库分支名称一致，或者在推送时指定正确的远程分支名称。当前本地仓库默认主分支为master，github仓库默认为main。可以使用 git push origin master:main将本地文件push到github仓库中。
- 如果远程仓库分支包含你本地没有的提交，可能需要先拉取远程分支的更改。

### 解决冲突

- 如果在推送时遇到冲突，可以使用 `git pull` 来合并远程分支的更改，或者使用 `git push -f` 强制推送（慎用，会覆盖远程分支的提交）。

### 其他常用命令

- **git status**: 查看当前仓库的状态。
- **git log**: 查看提交历史。
- **git diff**: 查看文件更改详情。
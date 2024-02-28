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

cloudflare page上面有一些奇怪的问题, 于是我尝试了将hexo 博客部署到自己的服务器上, 参考下面的博客
https://byer.top/posts/63e61cee.html
由于我的vps需要通过密钥连接, 按照上面博客无脑操作会报错, 我在本地电脑添加了一些配置:
确保私钥格式正确：Git和大多数SSH客户端期望使用OpenSSH格式的私钥。如果你的私钥是PuTTY私钥格式（.ppk），你可能需要使用PuTTY的puttygen工具将其转换为OpenSSH格式。

配置SSH：在你的用户目录下（通常是C:\Users\<你的用户名>\），找到或创建一个.ssh目录，并在其中创建一个名为config的文件（没有文件扩展名）。

在这个config文件中，添加如下配置（根据你的实际路径修改密钥路径）：
```bash
Host 142.171.xxx.xxx
    HostName 142.171.xxx.xxx
    User root
    IdentityFile C:/Users/Administrator/Downloads/mobaxterm-private-key/1002
    IdentitiesOnly yes
```


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

### 补充
转载自https://byer.top/posts/63e61cee.html
安装 Git
apt-get install git #Debian/Ubuntu
yum install git #Fedora/RedHat/CentOS
创建远程仓库
通过如下代码我们创建一个空的仓库来提交代码。

mkdir /root/git/ #可以自定义为自己的路径

修改文件夹权限
chown -R $USER:$USER /root/git/
chmod -R 755 /root/git/

创建远程 GIT 仓库
cd /root/git
git init --bare blog.git #可以改为自己的仓库名
创建 Git 钩子
执行以下的代码，在 blog.git/hooks 目录下创建一个新的钩子文件：

vim /root/git/blog.git/hooks/post-receive

打开文件后，加入下面的代码：
git --work-tree=/opt/1panel/apps/openresty/openresty/www/sites/blog/index --git-dir=/root/git/blog.git checkout -f
# --work-tree的值是你的前端代码保存的目录，我这里使用的1Panel，使用其他的可以自己修改文件目录
# 使用1Panel需要先创建网站，然后在创建文件钩子

使用以下方法保存你的文件，先按下 ESC，输入:wq!，回车即可保存
将文件保存后，修改文件权限
chmod +x /root/git/blog.git/hooks/post-receive
修改_config.yml
找到_config.yml 的 deploy 一行，修改为如下代码

deploy:
  - type: git
    git: 
      site: ssh://root@ip:22/root/git/blog.git

最后使用 hexo d 即可将其推送到自己的服务器上。
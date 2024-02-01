---
title: docker以及docker-compose的一些常用命令
date: 2024-01-26 00:48:52
tags:
    - "Linux"
    - "Docker"
thumbnail: https://cdn.jsdelivr.net/gh/ZefanHu/blogImage@main/img/20240126005128.png
---


Docker和Docker Compose是两个非常强大的工具，它们可以帮助你管理和部署应用。以下是一些常用的命令：

一键安装docker:

```
 curl -fsSL https://get.docker.com | bash -s docker
```

一键安装docker-compose:

```
apt install docker-compose
```

### Docker 常用命令

- `docker run`：运行一个Docker容器。例如，`docker run -d -p 8080:80 --name myapp myimage`会以后台模式运行一个名为`myapp`的容器，该容器基于`myimage`镜像，并将容器的80端口映射到主机的8080端口。
- `docker ps`：列出正在运行的Docker容器。添加`-a`参数（即`docker ps -a`）可以列出所有容器，包括已停止的。
- `docker stop`：停止一个正在运行的Docker容器。例如，`docker stop myapp`会停止名为`myapp`的容器。
- `docker rm`：删除一个Docker容器。例如，`docker rm myapp`会删除名为`myapp`的容器。
- `docker images`：列出本地的Docker镜像。
- `docker rmi`：删除一个Docker镜像。例如，`docker rmi myimage`会删除名为`myimage`的镜像。
- `docker pull`：从Docker Hub下载一个Docker镜像。例如，`docker pull nginx`会下载nginx镜像。
- `docker build`：根据Dockerfile构建一个Docker镜像。例如，`docker build -t myimage .`会在当前目录下根据Dockerfile构建一个名为`myimage`的镜像。

### Docker Compose 常用命令

- `docker-compose up`：根据`docker-compose.yml`文件启动服务。添加`-d`参数（即`docker-compose up -d`）可以在后台运行。
- `docker-compose down`：停止并删除`docker-compose.yml`文件定义的所有服务。
- `docker-compose ps`：列出`docker-compose.yml`文件定义的所有服务的状态。
- `docker-compose logs`：查看`docker-compose.yml`文件定义的所有服务的日志。
- `docker-compose build`：根据`docker-compose.yml`文件构建服务。

### Docker-Compose安装Nginx Proxy Manager

首先请确保您已经安装Docker-Compose，然后创建一个目录，用来保存Nginx Proxy Manager数据：

```bash
mkdir nginx && cd nginx
```

在刚刚创建的目录下新建一个文件,命名为`docker-compose.yaml`，然后复制下面的内容进行保存。

```bash
version: '3.8'
services:
  app:
    image: 'jc21/nginx-proxy-manager:latest'
    restart: unless-stopped
    ports:
      - '80:80'
      - '81:81'
      - '443:443'
    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
```

- 默认用户名为：admin@example.com
- 默认密码为：changeme

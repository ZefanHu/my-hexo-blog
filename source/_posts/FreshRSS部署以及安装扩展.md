---
title: FreshRSS部署以及安装扩展
date: 2024-02-02 11:04:39
tags:
    - "FreshRSS"
    - "Docker"
thumbnail: https://cdn.jsdelivr.net/gh/ZefanHu/blogImage@main/img/20240202113544.png
---

# 在Docker中部署FreshRSS及安装扩展指南

本指南将引导你通过Docker在你的服务器上部署FreshRSS，以及如何安装和启用扩展，以增强你的RSS阅读体验。作为示例，我们将安装YouTube/PeerTube Video Feed扩展。

## 准备工作

1. **创建工作目录**：首先，创建一个文件夹用于存放所有相关的Docker配置文件。例如`freshrss-docker`。

   ```bash
   mkdir freshrss-docker
   cd freshrss-docker
   ```

2. **下载Docker Compose文件**：接下来，下载以下Docker Compose配置文件到你的工作目录中。

   - [`docker-compose.yml`](https://github.com/FreshRSS/FreshRSS/blob/edge/Docker/freshrss/docker-compose.yml)
   - [`docker-compose-db.yml`](https://github.com/FreshRSS/FreshRSS/blob/edge/Docker/freshrss/docker-compose-db.yml)
   - [`docker-compose-local.yml`](https://github.com/FreshRSS/FreshRSS/blob/edge/Docker/freshrss/docker-compose-local.yml)

### 修改端口设置

- 在`docker-compose.yml`中，将`ports`节中的`port`替换为你希望FreshRSS使用的端口号。

  ```yaml
  ports:
    - "自定义端口:80"  # 将"自定义端口"更改为你的端口号
  ```

- 在`docker-compose-local.yml`中，同样替换`ports`节中的`port`。

  ```yaml
  ports:
    - "${PUBLISHED_PORT:-自定义端口}:${LISTEN:-80}"  # 将"自定义端口"更改为你的端口号
  ```

## 部署FreshRSS

使用以下命令来部署和管理FreshRSS：

- **更新Docker镜像**

  ```bash
  docker-compose -f docker-compose.yml -f docker-compose-db.yml pull
  ```

- **启动FreshRSS**

  ```bash
  docker-compose -f docker-compose.yml -f docker-compose-db.yml -f docker-compose-local.yml up -d --remove-orphans
  ```

- **查看日志**

  ```bash
  docker-compose -f docker-compose.yml -f docker-compose-db.yml logs -f --timestamps
  ```

## 访问FreshRSS

部署完成后，通过浏览器访问`http://你的IP:自定义端口/`。如果一切顺利，你将看到FreshRSS的初次引导界面。

## 安装扩展

**以YouTube/PeerTube Video Feed扩展为例**

1. **下载并解压扩展**

   ```bash
   wget https://github.com/kevinpapst/freshrss-youtube/archive/master.zip
   unzip master.zip
   ```

2. **复制扩展到容器**

   使用`docker cp`命令将扩展复制到容器中。请替换`path/to/freshrss-youtube-master/xExtension-YouTube`为实际路径。

   ```bash
   docker cp path/to/freshrss-youtube-master/xExtension-YouTube 容器ID:/var/www/FreshRSS/extensions/
   ```

3. **确认扩展安装成功**

   ```bash
   docker exec 容器ID ls /var/www/FreshRSS/extensions/
   ```

4. **重启FreshRSS容器**

   ```bash
   docker restart 容器ID
   ```

## 启用扩展

登录到FreshRSS的管理界面，在扩展部分找到并启用新安装的扩展。
![](https://cdn.jsdelivr.net/gh/ZefanHu/blogImage@main/img/20240202135041.png)
---
title: serverstatus-rust快速部署服务端及客户端
date: 2024-02-07 00:51:52
tags: 
    - "serverstatus"
thumbnail: https://cdn.jsdelivr.net/gh/ZefanHu/blogImage@main/img/20240212004901.png
---

# ServerStatus-Rust 安装配置指南

本文档提供了ServerStatus-Rust的安装和配置步骤，包括服务端和客户端的设置。请根据您的操作系统和架构遵循以下步骤。

## IPv4支持

如果您的服务器仅支持IPv6，需要先获取IPv4访问能力，可以使用WARP：

```bash
wget -N https://gitlab.com/fscarmen/warp/-/raw/main/menu.sh && bash menu.sh
```

## 服务端及客户端一键安装

适用于CentOS/Debian/Ubuntu x86_64的服务端安装步骤如下：

创建安装目录并切换到该目录：

```bash
mkdir -p /opt/ServerStatus && cd /opt/ServerStatus
```

安装unzip工具：

Debian/Ubuntu系统：

```bash
apt install -y unzip
```

CentOS系统：

```bash
yum install -y unzip
```

下载并执行安装脚本：

```bash
wget --no-check-certificate -qO one-touch.sh 'https://raw.githubusercontent.com/zdz/ServerStatus-Rust/master/one-touch.sh'
```

```bash
bash -ex one-touch.sh
```

根据自己的需求修改 `config.toml` 配置文件，本地修改配置文件参考(https://raw.githubusercontent.com/zdz/ServerStatus-Rust/master/config.toml):

```bash
nano /opt/ServerStatus/config.toml
```

重启服务端服务， 后面客户端每次配置要和服务端配置保持对应，后续更新客户端配置服务端也要注意:

```bash
systemctl restart stat_server
```

部署完毕后，通过以下地址访问ServerStatus-Rust的Web界面：

```bash
http://127.0.0.1:8080/ 或 http://<你的IP>:8080/
```

## 仅安装客户端

客户端的安装和配置步骤如下：

创建安装目录并切换到该目录：

```bash
mkdir -p /opt/ServerStatus && cd /opt/ServerStatus
```

根据您的操作系统安装unzip工具（参见服务端安装步骤）。

下载客户端软件：

```bash
wget --no-check-certificate -qO "client-x86_64-unknown-linux-musl.zip" "https://github.com/zdz/ServerStatus-Rust/releases/download/$(curl -m 10 -sL "https://api.github.com/repos/zdz/ServerStatus-Rust/releases/latest" | grep "tag_name" | head -n 1 | awk -F ":" '{print $2}' | sed 's/\"//g;s/,//g;s/ //g')/client-x86_64-unknown-linux-musl.zip"
```

解压缩客户端软件：

```bash
unzip -o "client-x86_64-unknown-linux-musl.zip"
```



配置systemd服务，`systemctl`是`systemd`系统和服务管理器的命令行工具，它是现代Linux发行版的标准部分，用于管理系统和应用程序的服务。你可以使用`systemd`来将`stat_client`作为守护进程（daemon）运行，这样即使关闭终端窗口，`stat_client`也会继续运行。

编辑 `/etc/systemd/system/stat_client.service`文件 ,将其中的服务器地址修改为您的服务器IP或域名, h2改为服务端对应配置的账号，p2改为对应密码。

```bash
nano /etc/systemd/system/stat_client.service
```



```bash
[Unit]
Description=Server Status Client
After=network.target

[Service]
Type=simple
User=root
ExecStart=/opt/ServerStatus/stat_client -a "http://服务器地址:8080/report" -u h2 -p p2
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

保存文件并退出文本编辑器后，重新加载`systemd`以识别新的服务单元文件，然后启用并启动`stat_client`服务：

```bash
sudo systemctl daemon-reload
sudo systemctl start stat_client
```

注意：如果您之前运行过此程序，请先停止旧服务再启动新服务以避免出错，这将会停止`stat_client`服务，并杀死与之关联的进程。

```bash
sudo systemctl stop stat_client
```

如果你需要对服务配置进行更改（例如，编辑`/etc/systemd/system/stat_client.service`文件以修改命令行参数），请在重新启动服务之前进行。做完任何必要的更改后，使用以下命令重新启动服务：

```bash
sudo systemctl start stat_client
```

如果你没有对服务配置进行更改，只是想重新启动服务，也可以使用`restart`命令直接停止并重新启动服务：

```bash
sudo systemctl restart stat_client
```

设置开机自启：

```bash
systemctl enable stat_client
```

检查客户端状态，确保服务正在运行且无报错：

```bash
systemctl status stat_client
```



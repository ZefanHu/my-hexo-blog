---
title: 配置ufw防火墙
date: 2024-02-11 00:50:54
tags: 
    - "Linux"
thumbnail: https://cdn.jsdelivr.net/gh/ZefanHu/blogImage@main/img/20240212004658.png
---
# 配置ufw防火墙

# 在Debian 12上开启UFW的步骤

在Debian 12操作系统上，使用UFW（Uncomplicated Firewall）是管理防火墙规则的一种简单方式。下面的步骤会指导你如何安装、配置和管理UFW。

## 安装UFW

如果你的系统还未安装UFW，可以通过以下命令来安装：

```bash
sudo apt update
sudo apt install ufw
```

## 启用UFW

安装完成后，你可以通过以下命令来启用UFW，这会将UFW设置为开机自启：

```bash
sudo ufw enable
```

## 设置默认规则（可选步骤）

为了提高系统安全性，建议设置默认规则来拒绝所有传入连接，同时允许所有传出连接：

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

## 添加规则

你可以根据需要添加规则来允许或拒绝特定的服务或端口。例如，允许SSH连接：

```bash
sudo ufw allow ssh
```

或者，如果你知道服务的端口号，也可以直接通过端口号来允许连接：

```bash
sudo ufw allow 22
```

## 开启HTTP和HTTPS端口

开启HTTP端口（80）：

```bash
sudo ufw allow http
```

开启HTTPS端口（443）：

```bash
sudo ufw allow https
```

## 检查UFW状态

你可以随时通过以下命令来检查UFW的状态，这有助于了解哪些规则是活动的：

```bash
sudo ufw status
```

## 重新启动UFW（如果需要）

如果你在配置规则后需要重新启动UFW来应用这些更改，可以使用以下命令：

```
sudo ufw disable
sudo ufw enable
```

请确保在配置防火墙规则时明确你的操作，错误的配置可能会导致远程锁定。
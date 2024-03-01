---
title: 使用rclone挂载onedrive扩展服务器空间
date: 2024-02-23 17:56:38
tags: 
    - "onedrive"
    - "linux"

thumbnail: https://cdn.jsdelivr.net/gh/ZefanHu/blogImage@main/img/202402241053828.png
---

## Rclone简介

Rclone *("rsync for cloud storage")* is a command-line program to sync files and directories to and from different cloud storage providers.

参考文档
[https://rclone.org/docs/](https://rclone.org/docs/)

本文尝试使用rclone将ondrive以磁盘的方式挂载到vps上。其他云存储如google drive、Amazon Drive等也可以自行尝试。

主要有以下几个步骤

## 在本地下载rclone并获取Token

在Windows电脑上下载Rclone，下载地址:[https://rclone.org/downloads/](https://rclone.org/downloads/) ，然后解压，使用cmd进入解压后的文件夹。运行命令

```bash
 `rclone.exe authorize "onedrive"
```

然后会跳转到浏览器的登录页面，登录并授权成功后cmd命令框就会返回你的token，注意不要登错账号了。保存好{xxxxxxxxxxx}备用。

![](https://cdn.jsdelivr.net/gh/ZefanHu/blogImage@main/img/202402251147748.png)

![](https://cdn.jsdelivr.net/gh/ZefanHu/blogImage@main/img/202402251148789.png)

![](https://cdn.jsdelivr.net/gh/ZefanHu/blogImage@main/img/202402251150323.png)

## vps上安装并配置rclone

运行以下命令，下载并安装Rclone

```bash
curl https://rclone.org/install.sh | sudo bash
```

安装完成后输入：

```bash
rclone config
```

输入 `n` 新建配置。

- `name` 可以随便输入
- 输入序号，选择onedrive

![](https://cdn.jsdelivr.net/gh/ZefanHu/blogImage@main/img/202402251152649.png)

![](https://cdn.jsdelivr.net/gh/ZefanHu/blogImage@main/img/202402251153407.png)

- `client_id` 和`client_secret` 直接回车默认即可。也可以进入 Microsoft Azure 应用注册页面，新注册一个，并添加需要的权限，如果使用自己的id, 请确保配置正确，不然最后会挂载失败。
- 注意这一步输入`n`，然后将第一步获取的token粘贴进去。其他的默认即可

Use web browser to automatically authenticate rclone with remote?
\* Say Y if the machine running rclone has a web browser you can use
\* Say N if running rclone on a (remote) machine without web browser access
If not sure try Y. If Y failed, try N.
y) Yes (default)
n) No
y/n> n

![](https://cdn.jsdelivr.net/gh/ZefanHu/blogImage@main/img/202402251155081.png)

验证一下是否能正常使用，`bufan`换成你的Name注意后面还有一个冒号

![](https://cdn.jsdelivr.net/gh/ZefanHu/blogImage@main/img/202402251156883.png)



```bash
touch test && rclone move test bufan:
```

![](https://cdn.jsdelivr.net/gh/ZefanHu/blogImage@main/img/202402251157144.png)

## 最后将onedrive挂载到指定路径

创建挂载点`mkdir xxx/xxxx/你的文件夹` 
使用下面的命令挂载：

```bash
rclone mount <配置的云盘名称>:<要挂载的云盘目录> <作为挂载点的本地目录> --copy-links --no-gzip-encoding --no-check-certificate --allow-other --allow-non-empty --umask 000 --daemon
```
比如我用的代码：
rclone mount bufan:/racknerd  /root/bufandrive  --copy-links --no-gzip-encoding --no-check-certificate --allow-other --allow-non-empty --umask 000 --daemon


输入df -h查看是否挂载成功。

![](https://cdn.jsdelivr.net/gh/ZefanHu/blogImage@main/img/202402251204036.png)

如果你安装了管理面板可以直接在面板状态看到对应云盘，下面是1panel

![](https://cdn.jsdelivr.net/gh/ZefanHu/blogImage@main/img/202402251206225.png)

到此为止，我们就成功挂载了，但是如果重新启动vps, 需要重新执行第三步的挂载命令重新挂载。

如果报错failed to mount FUSE fs: fusermount: exec: "fusermount3": executable file not found in $PATH
系统中找不到 fusermount3 这个必需的执行文件。fusermount3 是 FUSE (Filesystem in Userspace) 的一个组成部分，它允许非特权用户创建和管理自己的文件系统，而 rclone mount 命令需要 FUSE 来将云存储服务挂载为文件系统。对于 Debian/Ubuntu

```bash
sudo apt update
sudo apt install fuse3
```

## 补充

我们可以使用 `systemd` 的系统，创建一个 `systemd` 服务单元文件来管理 `rclone` 挂载。这不仅可以在启动时自动挂载，而且还可以在挂载失败时自动重试。

要创建一个 `systemd` 服务单元文件来管理 `rclone` 挂载，你需要创建一个新的服务文件，并将其放置在 `/etc/systemd/system` 目录下。以下是一个服务单元文件的示例，它将根据你提供的命令自动挂载 `bufan:/` 到 `/data/bufan`：

1. 打开一个终端或连接到你的 VPS。
2. 使用文本编辑器（如 `nano` 或 `vim`）创建服务文件。例如，使用以下命令创建一个名为 `rclone-mount.service` 的文件：

```bash
sudo nano /etc/systemd/system/rclone-mount.service
```

3. 在打开的编辑器中，粘贴以下内容：

```ini
[Unit]
Description=RClone Service
After=network.target

[Service]
Type=simple
User=root
ExecStart=/usr/bin/rclone mount bufan:/ /data/bufan \
  --copy-links \
  --no-gzip-encoding \
  --no-check-certificate \
  --allow-other \
  --allow-non-empty \
  --umask 000 \
  --daemon
ExecStop=/bin/fusermount -u /data/bufan
Restart=on-abort

[Install]
WantedBy=default.target
```

注意：

- `ExecStart` 包含了你的 `rclone` 挂载命令。
- `ExecStop` 使用 `fusermount -u` 命令来卸载挂载点，这是在服务停止时需要执行的。
- 确保 `/usr/bin/rclone` 是 `rclone` 的正确路径。如果不是，请根据你的系统路径进行调整。
- `Restart=on-abort` 指示如果服务异常终止，系统应尝试重启服务。

4. 保存并关闭文件。
5. 重新加载 `systemd` 以使新的服务单元文件生效：

```bash
sudo systemctl daemon-reload
```

6. 启用服务，使其在启动时自动运行：

```bash
sudo systemctl enable rclone-mount.service
```

7. 现在，你可以使用以下命令来启动、停止或查看服务的状态：

- 启动服务：`sudo systemctl start rclone-mount.service`
- 停止服务：`sudo systemctl stop rclone-mount.service`
- 查看服务状态：`sudo systemctl status rclone-mount.service`

这样，每次你的 VPS 启动时，`rclone` 挂载就会自动进行，而不需要手动重新执行挂载命令。


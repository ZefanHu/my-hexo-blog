---
title: 创建SSH密钥并将RackNerd登录方式设置为密钥认证
date: 2024-02-11 00:50:31
tags: 
    - "Linux"

thumbnail: https://cdn.jsdelivr.net/gh/ZefanHu/blogImage@main/img/20240212004434.png
---

# 创建SSH密钥并将RackNerd登录方式设置为密钥认证

在这篇博客中，我们将详细介绍如何生成SSH密钥，并将其应用于RackNerd VPS，以实现更安全的密钥认证登录方式。通过以下步骤，您可以轻松设置并提高服务器的安全性。

## 1. 生成密钥

### 使用Termius生成密钥

Termius提供了一个内置的SSH密钥生成器，非常适合在移动设备上使用。

- 打开Termius，点击右下角的小齿轮图标进入`Settings`。
- 选择`Keychain`，然后点击旁边的`key`图标旁的箭头，选择`Generate Key`。

### 使用MobaXterm或Puttygen

如果您更倾向于在PC上操作，MobaXterm和Puttygen是两个非常好的选择。

- 打开MobaXterm或Puttygen。
- 点击`Generate`按钮，根据提示在窗口内进行随机的鼠标点击或键盘动作以生成密钥。
- 密钥生成后，点击`Save private key`将私钥保存到您的计算机上。公钥则会显示在界面上。

> **提示：** 为了之后可能的需要，您可以通过加载私钥来重新生成公钥。

## 2. 配置RackNerd服务器

### 上传公钥

- 首先，使用常规的方式登录到您的VPS，并切换至root用户。

- 进入root用户的`.ssh`目录：

  ```bash
  cd ~/.ssh
  ```

- 如果目录不存在，使用以下命令创建它：

  ```
  mkdir -p ~/.ssh && cd ~/.ssh
  ```

- 使用nano编辑器打开`authorized_keys`文件，并将您的公钥粘贴进去：

  ```
  nano authorized_keys
  ```

### 修改SSH配置

- 接下来，需要修改SSH配置文件以启用密钥认证登录：

  ```
  sudo nano /etc/ssh/sshd_config
  ```

- 找到`PermitRootLogin`项，将其值更改为`without-password`，禁止root用户使用密码登录。

- 将`PubkeyAuthentication`的值更改为`yes`，启用公钥认证。

- 将`PasswordAuthentication`的值更改为`no`，禁用密码认证。

- 修改完成后，保存文件并退出编辑器。

### 重启SSHD服务

- 最后，重启sshd服务以应用配置更改：

  ```
  sudo service sshd restart
  ```

> **注意：** 在关闭当前SSH会话之前，请尝试在新窗口中使用密钥登录，以确保新配置生效且无误。

## 3. 使用密钥登录

至此，您的RackNerd服务器已经配置完成，可以通过SSH密钥进行安全登录了。使用SSH客户端时，确保指定了之前生成的私钥文件，这样就可以实现免密码登录。
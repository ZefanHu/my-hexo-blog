---
title: Cloudflare Zero Trust 保护网站后台登录
date: 2024-02-01 09:49:00
tags:
    - "Linux"
    - "Cloudflare"
thumbnail: https://cdn.jsdelivr.net/gh/ZefanHu/blogImage@main/img/20240201100608.png
---

由于我部署了Telegraph-Image图床，出于安全和便捷性考虑，下面介绍了结合Cloudflare Access使用GitHub账户进行登录保护的整个过程。

### 部署Telegraph-Image图床

首先，我部署了[Telegraph-Image图床](https://github.com/cf-pages/Telegraph-Image)，这是一个开源项目，可以方便地存储和管理图片资源。

为了访问后台管理页面，我决定使用Cloudflare Access结合GitHub账户进行安全登录。

### 使用Cloudflare Access保护后台登录

接下来，我将详细介绍如何通过Cloudflare Access保护网站后台登录的步骤：

#### 1. 在GitHub中创建Organization

- 访问GitHub首页，进入`Setting` -> `Organizations`，创建一个新的Organization，并记下Organization的名称。
![](https://cdn.jsdelivr.net/gh/ZefanHu/blogImage@main/img/20240201130600.png)

#### 2. 配置Cloudflare Access

- 登录Cloudflare，进入`Cloudflare Zero Trust` -> `Access` -> `Access Groups`，添加一个新的访问组。

#### 3. 在GitHub中创建OAuth App

- 回到GitHub，进入`Setting` -> `Developer setting` -> `OAuth App`，创建一个新的app。
- 将`Homepage URL`设置为`https://<your-Group-name>.cloudflareaccess.com`。
- `Authorization callback URL`设置为`Homepage URL/cdn-cgi/access/callback`。
- 创建一个Client secrets，并记下Client ID和secrets。
![](https://cdn.jsdelivr.net/gh/ZefanHu/blogImage@main/img/20240201102552.png)

#### 4. 在Cloudflare中添加登录验证方式

- 在Cloudflare中，进入`Settings` -> `Authentication` -> `Login Methods`，选择GitHub作为登录方式。
- 填写之前记下的Client ID和secrets，然后保存。
![](https://cdn.jsdelivr.net/gh/ZefanHu/blogImage@main/img/20240201104326.png)

#### 5. 完成设置并授权

- 点击`finish setup`，在GitHub中给新创建的Organization授权，点击`Grant`，然后`authorize`。
![](https://cdn.jsdelivr.net/gh/ZefanHu/blogImage@main/img/20240201131801.png)
![](https://cdn.jsdelivr.net/gh/ZefanHu/blogImage@main/img/20240201131832.png)

#### 6. 配置Cloudflare Access应用并创建额外的访问规则

- 在Cloudflare Access中，选择`add an application`，选择`self-hosted`。
- 配置相应的后台登录URL路径。
- 创建额外的访问规则：
  - `include`：用户满足任一条件即可访问。
  - `require`：用户必须满足所有条件才能访问。
  - `exclude`：排除特定条件的用户。

其他设置保持默认即可。通过以上步骤，我成功地使用Telegraph-Image图床，并通过Cloudflare Access保护了后台管理页面的安全。这不仅提高了安全性，还使得管理变得更加便捷。
输入上面受保护的后台登录URL路径，验证登录即可。
![](https://cdn.jsdelivr.net/gh/ZefanHu/blogImage@main/img/20240201131350.png)

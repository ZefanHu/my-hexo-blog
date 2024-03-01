---
title: Typora + PicList 自定义命令上传图片并实现加速
date: 2024-03-01 17:16:13
tags: 
    - "Typora"
    - "PicList"

thumbnail: https://work.aptzone.cc/https://alist.aptzone.cc/d/GoogleDrive/202403011718779.png
---

在使用Typora编辑器时，您可能遇到因网络原因导致图片加载缓慢或失败的问题。本文将介绍如何通过配置PicList和使用Cloudflare的CDN服务来加速图片访问，从而解决这一问题。下面是网络不少加载失败的图片：

![image-20240301162308301](https://work.aptzone.cc/https://alist.aptzone.cc/d/GoogleDrive/202403011623752.png)

## 解决方案概述

为了提高图片的访问速度，我们将利用Cloudflare的免费CDN服务。操作步骤主要包括：

1. 搭建Cloudflare Workers/Pages。
2. 自定义域名设置。
3. 在Typora中将上传服务设定为自定义脚本。
4. （可选）在PicList中设置自定义链接格式。

## 搭建Cloudflare Workers/Pages

首先，您需要创建一个Cloudflare Workers/Pages服务，这可以通过以下链接中的指南完成：

[这里有一个符合要求的易于自行搭建的项目](https://github.com/Rongronggg9/rsstt-img-relay/blob/main/README_zh-CN.md)

## 自定义域名

由于`workers.dev`域名在某些地区可能被屏蔽，建议使用`pages.dev`或自定义域名以提高稳定性。GitHub学生认证可免费获取一年的`.me`域名。

![image-20240301165230976](https://work.aptzone.cc/https://alist.aptzone.cc/d/GoogleDrive/202403011652573.png)

## Typora上传服务设定为自定义脚本

您需要在Typora中设置上传服务为自定义脚本。这里提供了PowerShell和bash两种脚本的示例：(二选一，确保系统中安装了对于工具，对于bash脚本需要curl和jq，curl一般windows自带，jq可能需要手动安装，参考https://jqlang.github.io/jq/download/)

编写PowerShell脚本，比如`upload_typora_piclist.ps1`：

```powershell
# 定义请求的URI
$uri = "http://127.0.0.1:36677/upload"

# 如果POST请求需要上传文件或其他数据，这里需要调整
# 例如，上传文件，需要构造表单数据或直接使用文件内容
# $body = @{file = Get-Content 'path/to/your/file' -Raw}
$body = @{}

# 发送POST请求
$response = Invoke-RestMethod -Uri $uri -Method Post -Body $body -ContentType "application/json"

# 检查是否上传成功
if ($response.success -eq $true) {
    # 构造并显示最终结果
    $resultUrl = "你的Cloudflare自定义域名" + $response.result[0]
    Write-Output "Upload Success:"
    Write-Output $resultUrl
} else {
    Write-Output "Upload Failed"
}
```

bash脚本，比如`upload_typora_piclist_sh.sh`：

```bash
#!/bin/bash

# 目标URL
URL="http://127.0.0.1:36677/upload"

# 发送POST请求并获取响应，这里假设不需要传递具体数据
# 如果需要上传文件，你可能需要使用-F 参数
response=$(curl -s -X POST "$URL" -H "Content-Type: application/json")

# 使用jq处理JSON响应，提取需要的信息
success=$(echo $response | jq -r '.success')
if [[ $success == "true" ]]; then
    resultUrl=$(echo $response | jq -r '.result[0]')
    echo "Upload Success:"
    echo "你的Cloudflare自定义域名$resultUrl"
else
    echo "Upload Failed"
fi
```



![image-20240301170016739](https://work.aptzone.cc/https://alist.aptzone.cc/d/GoogleDrive/202403011700249.png)

## PicList设置自定义链接格式

在PicList中，您可以设置自定义链接格式以使用Cloudflare CDN进行加速。例如，将链接格式修改为`https://xxxx.xxxxxx.workers.dev/$url`。之后，当您从相册中复制链接时，选择`Custom`选项，即可获取经过Cloudflare CDN加速的链接。

![202403011702963](https://work.aptzone.cc/https://alist.aptzone.cc/d/GoogleDrive/202403011702963.png)



![202403011703863](https://work.aptzone.cc/https://alist.aptzone.cc/d/GoogleDrive/202403011703863.png)


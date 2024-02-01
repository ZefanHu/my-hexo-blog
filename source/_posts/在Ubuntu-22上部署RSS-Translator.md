---
title: 在Ubuntu 22上部署RSS-Translator
date: 2024-01-26 00:26:34
tags:
    - "Linux"
    - "RSS"
thumbnail: https://cdn.jsdelivr.net/gh/ZefanHu/blogImage@main/img/20240126003912.png
---

### 创建目录和下载脚本

首先，我们需要创建一个新的目录来存放下载的脚本。可以使用以下命令：

```
mkdir -p ~/rss-translator
cd ~/rss-translator
```

然后，我们使用`wget`命令下载`install_update.sh`脚本：

```
wget "https://raw.githubusercontent.com/rss-translator/RSS-Translator/main/deploy/install_update.sh"
```

### 赋予脚本执行权限并执行

接下来，我们需要赋予脚本执行权限：

```
sudo chmod +x install_update.sh
```

然后，执行脚本来安装或更新项目：

```
sudo ./install_update.sh
```


至此，基本的搭建就完成了。如果需要添加ssl，可以去查看[官方教程](https://github.com/rss-translator/RSS-Translator?tab=readme-ov-file#开启ssl)。
然后我们可以使用http://0.0.0.0:8000 访问网站，也可以配置反代域名访问。
![](https://cdn.jsdelivr.net/gh/ZefanHu/blogImage@main/img/20240126004127.png)

### 解决可能的问题

在使用过程中，可能会遇到一些问题。例如，使用nginx可能会报错`CSRF verification failed. Request aborted.`。这可能是由于Django在处理CSRF验证时，没有正确地获取到原始的主机名和来源。这可能是因为nginx在处理请求时，修改了这些值。在Django的设置中，添加`CSRF_TRUSTED_ORIGINS`变量，这个变量应该包含你的服务器的地址。如果应用使用了SSL，可能需要在Django的设置中，设置`CSRF_COOKIE_SECURE`为`True`。这个设置会让Django在处理带有SSL的请求时，使用安全的cookie。另外，我在改为监听ipv6之后，添加订阅似乎遇到了问题，后来我改回ipv4就可以正常使用了。

### 补充

需要注意的是，debain11默认python版本为python3.11，而ubuntu22默认版本为python3.10。因此，我们需要使用`python3`而不是`python`。如果要使用`python`，可能需要更改一些配置。

在Ubuntu和Debian系统中，你可以使用`update-alternatives`命令来实现。

1. 首先，需要检查系统中已经安装的Python版本。可以使用以下命令：

```
ls /usr/bin/python*
```

这个命令会列出所有在`/usr/bin/`目录下的以`python`开头的文件。能看到`python3.10`和`python3.11`。

1. 接下来，你需要使用`update-alternatives`命令来设置`python`的默认版本。首先，你需要为`python3.10`和`python3.11`创建`update-alternatives`条目。可以使用以下命令：

```
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.10 1
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.11 2
```

在这些命令中，`--install`选项用于创建新的条目，`/usr/bin/python`是链接，`python`是该链接的名字，`/usr/bin/python3.10`和`/usr/bin/python3.11`是目标文件，`1`和`2`是优先级。

1. 然后，你可以使用`update-alternatives`命令来更改默认的Python版本。可以使用以下命令：

```
sudo update-alternatives --config python
```

这个命令会列出所有的`python`条目，并让你选择一个作为默认版本。只需要输入你想要的版本对应的数字，然后按回车键即可。
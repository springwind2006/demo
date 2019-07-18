---
title: 安装Steeze
permalink: /docs/startup/install/
---

### 1. 获取Steeze
Steeze框架已托管在github上，地址是 <https://github.com/steezer/steeze> 
  
安装git后执行如下命令即可获取代码文件：

```
git clone https://github.com/steezer/steeze.git
```

也可以进入 <https://github.com/steezer/steeze/releases>下载最新版本  
  
下载完成并解压，进入系统框架根目录

### 2. 设置public目录
安装完成之后，需要将Web服务器的根目录指向public目录。该目录下的index.php文件将作为所有进入应用程序的 HTTP 请求的唯一入口。
### 3. 配置文件
框架的所有配置文件都放在 storage/Conf 目录中。除了route和middleware，其余所有的配置都可以在模块目录配置，模块中配置会覆盖storage/Conf目录下的同名配置键值。
### 4. 目录权限
安装完成之后，需要将storage目录设置为可读写
### 5. 优雅链接的配置
#### Apache
Steeze 使用 public/.htaccess 文件来为前端控制器提供隐藏了 index.php 的优雅链接。如果你的 Steeze 使用了 Apache 作为服务容器，请务必启用 mod_rewrite模块，让服务器能够支持 .htaccess 文件的解析。
如果 Steeze 附带的 .htaccess 文件不起作用，就尝试用下面的方法代替：

```
Options +FollowSymLinks
RewriteEngine On

RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^ index.php [L]
```
#### Nginx
如果你使用的是 Nginx，在你的站点配置中加入以下内容，它将会将所有请求都引导到 index.php 前端控制器：

```
location / {
    try_files $uri $uri/ /index.php?$query_string;
}
```

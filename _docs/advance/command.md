---
title: 命令行模式
permalink: /docs/advance/command/
---

### 1. 命令行程序的开发
在steeze中完美支持开发命令行程序，无论在任何目录下，只需要引入steeze框架根目录下的kernel/base.php文件，就可以使用系统提供的功能。  
下面是一个获取当前IP信息的例子：

```
<?php
include dirname(__FILE__).'./steeze/base.php';

$client=make('\Library\Client');
echo $client->getOnlineIp();
```
  
在app/console文件夹下，系统内置了一些命令行程序：

```
├── console
│   ├── crx2zip.php  //将chrome的crx插件转换为zip文件
│   ├── spider.php   //一个简单的网页爬虫
│   ├── swoole.php   //swoole模式运行服务
│   └── thumb.php    //批量图片处理程序
```
使用这些命令，只需要将当前目录切换到Steeze根目录，然后执行php脚本命令。   
例如：

```
php app/console/thumb.php
```
然后会显示相关用法：

```
This tool is help you to batch resize images
Usage: thumb [option]
option:
  -d | --dir  The directory to be processed (Required)
  -w | --width  Maximum width pix for resize, "0" for auto,default: 920
  -h | --height  Maximum height pix for resize,"0" for auto,default: 0
  -s | --save  The other directory to save, default is current directory
  -c | --cut  Cut type, for 0 without cut, for 1 shrink cut, for 2 zoom cut, default: "0"
  -t | --type Type of image to be processed, default "jpg", support for "jpg, png, jpeg, gif"
  -o | --output The output image type, default same as input
```

### 2. 命令行模式运行系统应用
位于steeze根目录下app文件夹中的完整应用，也可以使用命令行模式运行，  
运行方式如下：  
**php public/index.php [路由路径] [主机名]** 
     
例如运行“www.h928.com”域名下的 “/about”路由：

```
php public/index.php /about www.h928.com
```
等效于在浏览器端访问：

```
http://www.h928.com/about
```






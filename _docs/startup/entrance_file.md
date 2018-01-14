---
title: 入口文件
permalink: /docs/startup/entrance_file/
---

Steeze采用单一入口模式进行项目部署和访问，无论完成什么功能，一个应用都有一个统一（但不一定是唯一）的入口。  
所有应用都是从入口文件开始的，每个应用可以配置单独的入口文件，可以多个应用共享一个入口。  

### 入口文件的作用
 - 定义系统相关常量（可选）
 - 载入框架入口文件（必须）
 - 启动应用（必须）
   
在系统的public目录下，框架自带一个入口文件

```
<?php
define('ROOT_PATH', dirname(__FILE__).DIRECTORY_SEPARATOR);
include dirname(__FILE__).'/../kernel/base.php';
Loader::app();
```

如果你改变了项目目录（例如把app更改为application），只需要在入口文件更改APP_PATH常量定义即可：

```
<?php
define('ROOT_PATH', dirname(__FILE__).DIRECTORY_SEPARATOR);
define('APP_PATH', dirname(__FILE__).DIRECTORY_SEPARATOR.'application'.DIRECTORY_SEPARATOR);
include dirname(__FILE__).'/../kernel/base.php';
Loader::app();
```
**说明**: DIRECTORY_SEPARATOR常量为系统的目录分隔符，windows环境下为"\"，linux环境下为"/"，入口文件中定义的路径常量必须以目录分隔符（"/"或"\"）结尾。  
  
  
**入口文件支持常量的定义**    

<table>
    <tr><td><b>常量</b></td><td><b>描述</b></td></tr>
    <tr><td>APP_PATH</td><td>应用所在路径（默认系统框架下的“app”目录）</td></tr>
    <tr><td>DEFAULT_APP_NAME</td><td>默认应用名称（默认为“home”，配置为空可以使用单应用模式）</td></tr>
    <tr><td>ROOT_PATH</td><td>Public根目录</td></tr>
    <tr><td>VENDOR_PATH</td><td>外部库目录</td></tr>
    <tr><td>STORAGE_PATH</td><td>数据存储目录</td></tr>
    <tr><td>APP_DEBUG</td><td>应用调试模式（默认为“false”）</td></tr>
    <tr><td>STORAGE_TYPE</td><td>存储类型（默认为“File”）</td></tr>
</table>




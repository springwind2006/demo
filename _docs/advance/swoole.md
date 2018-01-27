---
title: Swoole模式
permalink: /docs/advance/swoole/
---

### 1. swoole的安装
swoole面向生产环境的PHP异步网络通信引擎，官方网址是：<a href="http://www.swoole.com" target="_blank">http://www.swoole.com</a>   
安装扩展：   
Linux环境

```
#!/bin/bash
pecl install swoole
```
Mac环境

```
#!/bin/bash
brew install swoole
```


### 2. steeze在swoole模式下运行

Steeze完美支持swoole的HttpServer应用，性能是传统php-fpm的数十倍，
<a href="https://wiki.swoole.com/wiki/page/326.html" target="_blank">点击查看更多关于swoole下HttpServer的介绍</a>。   
在框架的根目录下执行如下命令启动swoole服务器：

```
php app/console/swoole.php & 
```
然后在浏览器中访问：

```
http://127.0.0.1:9501/  
```
此时steeze框架下的应用运行在swoole模式下。

app/console/swoole.php文件是一个用php写的服务端程序，代码如下：

```
<?php
include dirname(__FILE__).'/../../kernel/base.php';
!class_exists('swoole_http_server',false) && 
	exit("Swoole server extension is not install,see: https://www.swoole.com/\n");
$http = new swoole_http_server("0.0.0.0", 9501);
$http->on('request', function ($request, $response) {
	$filename=ROOT_PATH.ltrim($request->server['request_uri']);
	if(is_file($filename)){
		$contentType=C('mimetype.'.fileext($filename),'application/octet-stream');
		$response->header('Accept-Ranges','bytes');
		$response->header('Content-Type',$contentType); // 网页字符编码
		$response->sendfile($filename);
	}else{
		Loader::app($request, $response);
	}
});
	
$http->start();
```
steeze与swoole整合的核心代码是：

```
Loader::app($request, $response);
```
将swoole的Request和Response对象传入steeze应用，从而实现让steeze来处理请求并将结果输出到用户浏览器。 
   
在实际的项目部署中，可以将app/console/swoole.php代码中默认的9501端口修改后运行多个服务，然后配合nginx实现多服务器负载均衡。

### 3. nginx+swoole配置
swoole可以和nginx配合使用，让nginx来处理静态的资源文件。   
配置nginx.conf如下：

```
server {
    root /data/wwwroot/;
    server_name local.swoole.com;

    location / {
        proxy_http_version 1.1;
        proxy_set_header Connection "keep-alive";
        proxy_set_header X-Real-IP $remote_addr;
        if (!-e $request_filename) {
             proxy_pass http://127.0.0.1:9501;
        }
    }
}
```

### 4. swoole模式运行下的应用开发注意事项
swoole模式本质上是在命令行下运行代码，但跟一般的命令行下的程序不一样，swoole模式是替代php-fpm的企业级解决方案。  
应用的开发方式跟php-fpm下运行的程序基本一样，但一些细节需要注意：  
1). 在代码中不能使用exit()函数，因为使用此函数会导致服务器程序整体退出。   
2). 在代码中向浏览器输出字符串使用Library\Response类提供的write方法（或者在控制器中直接返回字符串），如果使用echo或其它输出函数，会输出到终端日志文件中，而不是输出到用户浏览器端。   
3). 使用make函数在容器中构建的对象会一直内存中存在，因此容器功能实现了真正的单例模式，如果需要下次请求使用新对象用new关键字创建。  



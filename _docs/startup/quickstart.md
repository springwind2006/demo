---
title: 快速体验
permalink: /docs/startup/quickstart/
---

配置好运行环境之后，进入Storage/Conf目录，打开route.php文件  
输入如下代码：

```
<?php
return [
	'default' => [
		'/'=> function(){
			return 'Hello world!';
		},
	]
];
```
然后打开浏览器，访问：http://localhost/，此时输出“Hello world!”


也可以通过配置控制器的方式，进入app/Home/Controller目录  
新建一个控制器：

```
<?php
namespace App\Home\Controller;
use Library\Controller;

class Demo extends Controller{
	// 外部访问的方法名称
	public function hello(){
		return 'Hello world!';
	}
}
```
然后在Storage/Conf/route.php文件中配置：

```
<?php
return [
	'default' => [
		'/hello'=> 'Demo/hello@home',
	]
];
```
然后打开浏览器，访问：http://localhost/hello，此时输出“Hello world!”



---
title: 路由参数介绍
permalink: /docs/route/route_param/
---

在路由中可以配置参数，路由参数之后会按照名称注入到控制器（或Closure函数）路由处理器中。  
例如：

```
return [
	'default'=>[
		'/info/{userid}'=> 'user/info@home',
		'/list/{catid}/{page}'=> function($page,$catid){
			echo $catid.':'.page;
		},
	]
];
```
**注**：在以上实例中，由于$page和$catid参数是按照参数名称传入，因此参数传入顺序可以跟路由参数配置顺序不同。

Home模块下User控制器的info方法如下：

```
public function info($userid){
	echo $userid;
}
```

另外对于控制器路由处理器，可以使用路由参数指定，从而更加方便路由配置，  
如下的路由配置，当访问路径为"/user/list"时，直接访问Home模块下User控制器的list方法：

```
return [
	'default'=> [
		'/{c}/{a}'=> '{c}/{a}@home'
	]
];
```
可以使用“d”和“s”来限定参数的类型，“d”表示参数只能是数值，“s”表示参数是字符串  
下面的路由只匹配：“/test/”+“数值”，例如：/test/11

```
return [
	'default'=> [
		'/test/{page|d}'=> function($page){
			echo $page;
		}
	]
];
```
在参数后面使用“?”，可以将路由参数设置为可选，例如：

```
return [
	'default'=> [
		'/{c}/{a}/{page?}'=> '{c}/{a}@home',
		'/test/{page|d?}'=> function($page){
			echo $page;
		}
	]
];
```






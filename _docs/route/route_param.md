---
title: 路由参数介绍
permalink: /docs/route/route_param/
---

1、在路由中可以配置参数，路由参数之后会按照名称注入到控制器（或Closure函数）路由处理器中。  
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

2、另外对于控制器路由处理器，可以使用路由参数指定，从而更加方便路由配置，  
如下的路由配置，当访问路径为"/user/list"时，直接访问Home模块下User控制器的list方法：
```
return [
	'default'=> [
		'/{c}/{a}'=> '{c}/{a}@home'
	]
];
```

3、可以使用“d”和“s”来限定参数的类型，“d”表示参数只能是数值，“s”表示参数是字符串  
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

4、在参数后面使用“?”，可以将路由参数设置为可选，例如：  
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

5、在1.3.0版本之后，支持特定字符串分割成多个路由参数，例如：  
```
return [
	'default'=> [
		'/{c}/{a}/{catid|d}-{page|d}'=> '{c}/{a}@home',
		'/test/{catid}-{page|d}'=> function($page, $catid){
			echo $page;
		}
	]
];
```
例如：客户端访问/article/list/12-31，路由变量catid设置为12、page设置为31。   

6、在1.3.1版本之后，新增对客户端变量配置路由参数的支持，客户端参数只能位于以“#”分割的右侧，
多个客户端变量以“&”分割。
例如：  
```
return [
	'default'=> [
		'/{c}/{a}#p={page?}'=> '{c}/{a}@home', //获取POST（或GET）的p参数到路由参数page
		'/{c}/{a}#page={page?}&id={id|d?}'=> '{c}/{a}@home', //多个参数设置
		'/{c}/{a}#view={page?}_{id|d?}'=> '{c}/{a}@home', //获取同一个客户端参数值到多个路由参数
		'/test#{page|d?}'=> function($page){
			echo $page;
		}
	]
];
```
注意：如果客户端为POST方法，优先获取POST变量，然后获取GET变量

7、在1.3.2版本之后，新增默认路由设置支持，例如：  
```
return [
	'default'=> [
		'/{c}/{a}'=> '{c}/{a}@home', //获取POST（或GET）的p参数到路由参数page
		'/test#{page|d?}'=> function($page){
			echo $page;
		},
		'/**'=>'index/hello',  //默认路由
	]
];
```  
注意：默认路由位于所有配置的路由后面，否则默认路由后面的配置无效








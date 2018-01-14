---
title: 路由配置格式
permalink: /docs/route/config_format/
---

路由配置使用数组方式，键名为匹配的路由地址，以“/”开头，键值为路由处理器，可以是控制器或Closure函数。
### 1. 控制器处理器
控制器处理器需要指定控制器类和处理方法，格式采用“控制器类名/方法名@模块名”（不区分大小写）。  
一个简单的控制器处理器配置如下：

```
return [
	'www.abc.com'=>[
		'/demo'=> 'index/demo@home'
	]
];
```
表示当浏览器访问网址：http://www.abc.com/demo 时，将访问Home模块下的index控制器的demo方法  
也可以在域名上指定模块名称，例如：

```
return [
	'www.abc.com@home'=>[
		'/demo'=> 'index/demo',
		'/list'=> 'content/demo',
	]
];
```
### 2. Closure函数处理器
路由处理器也可以使用Closure函数，例如：

```
return [
	'www.abc.com@home'=>[
		'/demo'=> function(){
			return 'abc';
		},
		'/info'=> function(){
			return [
				'code'=>0,
				'message'=>'success'
			];
		},
		'show'=> function(){
			return view('index/show');
		}
	]
];
```
**特别提示**：控制器处理器和Closure函数的返回值可以是字符串、数组和视图：字符串直接输出到浏览器、数组以json字符串的方式输出、视图以渲染后的模板输出。







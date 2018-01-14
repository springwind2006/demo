---
title: 路由中间件
permalink: /docs/route/route_middleware/
---

可以在一组或单个路由中配置一个或多个中间件，多个中间件以“&”连接，这样当访问到匹配的路由时，依次执行完成中间件之后，再运行路由绑定的控制器方法或Closure处理器。
### 1. 单个路由的中间件的配置
中间件与控制器之间使用“>”连接，例如：

```
return [
	'default'=> [
		'/show'=> 'auth&convert>content/show',
	]
];
```
### 2. 一组路由的中间件的配置
多个路由使用数组，例如：

```
return [
	'default'=> [
		'auth&convert'=> [
			'/{c}/{a}'=>'{c}/{a}',
			'/{c}/{a}/{user|d}'=>'{c}/{a}',
		]
	]
];
```
   
中间件的开发和使用，参见[“高级特性/中间件”](/docs/advance/middleware/)







---
title: 路由中间件
permalink: /docs/route/route_middleware/
---

可以在一组或单个路由中配置一个或多个中间件，多个中间件以“&”连接，这样当访问到匹配的路由时，依次执行中间件。
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







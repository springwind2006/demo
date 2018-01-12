---
title: 配置类型
permalink: /docs/route/config_type/
---

### 1. 全局配置
全局路由配置位于storage/Conf/route.php文件中，全局路由配置中可以设置默认的路由，当系统根据主机名（域名）没有找到匹配域时，使用默认路由配置。  
默认路由配置示例：

```
return [
	'default' => [
		'/test'=> function(){
			return 'test';
		},
		'/list'=> 'content/list@home'
	]
];
```
除了默认的路由配置，可以根据特定域名配置，域名支持顶层子域名通配符，例如：

```
return [
	'm.steeze.cn' => [
		'/show'=>'index/show@mobile'
	],
	'*.steeze.cn' => [
		'/list'=>'index/list@mobile'
	],
];
```
对于特定域名配置，可以将模块名称绑定到域名上面，例如：

```
return [
	'm.steeze.cn@mobile' => [
		'/show'=>'index/show',
		'/info'=>'index/show@home'
	],
	'*.test.cn@home' => [
		'/list'=>'index/list',
		'/page'=>'index/page',
	],
];
```
同时支持将特定客户端访问（如GET或POST）方法绑定到路由  
如果不指定方法，默认使用GET方法，例如：

```
return [
	'm.steeze.cn@mobile' => [
		'POST:/show'=>'index/show',
		'/info'=>'index/show@home'
	],
];
```

**特别说明**：
- 1). 如果在路由处理器和域名绑定中同时指定模块名称，则优先使用路由控制器中指定的模块名称；
- 2). 找到匹配的域，但未找到匹配的路由时，系统不会使用默认路由匹配，而是提示未找到页面；
- 3). 如果未绑定模块，将使用系统默认模块名称；
- 4). 默认路由配置不支持直接绑定到特定模块，例如以下默认路由配置将无法使用：

```
return [
	'default@home' => [
		'/test'=> function(){
			return 'test';
		},
		'/list'=> 'content/list@home'
	]
];
```
默认路由绑定系统默认模块名，也可以使用“bind_module”环境变量指定，详情查看 [系统配置部分](/docs/config/index)


### 2. 独立域配置
位于storage/Routes/目录下，以匹配的域名为文件名  
例如：storage/Routes/m.steeze.com.php

```
return [
	'/info'=>'content/info@mobile'
];
```
也可以将模块直接绑定到域名上，这样配置文件路径为：storage/Routes/域名@模块名.php  
例如以上配置为：storage/Routes/m.steeze.com@mobile.php  
  
另外独立域配置也支持域名通配符，例如：storage/Routes/*.steeze.com@mobile.php






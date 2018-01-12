---
title: 全局配置
permalink: /docs/config/index/
---

全局配置是所有应用共享的配置，位于STORAGE_PATH/Conf目录中，所有的配置以返回数组的方式，键名区分大小写  
例如database（数据库配置）：

```
<?php
return [
	'default' => [
		'type'=>  env('db_type','mysql'),     // 数据库类型
		'host'=>  env('db_host','127.0.0.1'), // 服务器地址
		'name'=>  env('db_name','test'),          // 数据库名
		'user'=>  env('db_user','root'),      // 用户名
		'pwd'=>  env('db_pwd',''),          // 密码
		'port'=>  env('db_port','3306'),        // 端口
		'prefix'=>  env('db_prefix',''),    // 数据库表前缀
	],
	'lite' => [
		'type' => 'sqlite3',
		'dsn' => STORAGE_PATH.'Data'.DS.'lite_sqlite3.db',
		'prefix' => env('db_prefix','')
	]
];
```
默认的全局配置包括：  
- system 系统框架的配置
- database 数据库信息配置
- middleware 中间件配置
- route 路由配置
- mimetype 文件类型


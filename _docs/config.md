---
title: 系统配置
permalink: /docs/config/
---

## 一、全局配置
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

## 二、应用配置
应用配置位于应用目录下的Conf目录中，除了route（路由配置）、middleware（中间件配置）外，应用下的配置会覆盖全局中同名的配置。

## 三、配置的获取
使用单字符函数C直接获取配置，例如获取database（数据库配置）中的default键值，可以使用如下方式：

```
<?php
$default=C('database.default');
```
同时支持多维获取数组配置，例如获取（database）默认数据库配置的type类型，可以用如下方式：

```
<?php
$type=C('database.default.type');
```
如果是system（系统配置），可以省去配置名称，直接根据键值获取

例如从system（系统配置）中：

```
<?php
/*
 * 注意：本文件不支持路由常量配置
 */
return [
	// 常规设置
	'version' => '1.0.0', // 应用版本
	'charset' => 'utf-8', // 网站字符集

	// ......
];
```
获取网站默认字符集编码：

```
<?php
$type=C('charset');
```
## 四、配置的动态设置
在系统运行时期，可以动态设置配置：

```
<?php
C([
	'lang' => 'zh-cn',
	'debug' => 0, 
	'lock_ex' => 1,
],'test');
```
如果第二个参数不提供，默认为system（系统配置）。

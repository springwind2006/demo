---
title: 获取配置
permalink: /docs/config/get_config/
---

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

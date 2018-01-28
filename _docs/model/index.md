---
title: 模型层简介
permalink: /docs/model/index/
---

Steeze的模型参考了ThinkPHP3.2的<a target="_blank" href="http://document.thinkphp.cn/manual_3_2.html#define_model">Model层实现</a>，同时优化了配置参数、增强了部分功能实现。  
具体差异提现在如下几个方面： 
   
### 一、Model层的核心类实现了ArrayAccess接口
支持通过数组的方式访问内部数据集，例如以前通过直接赋值对象属性:

```
$user=M('user')->where('id=1');
$user->name='liming';
$user->year=23;
$user->save();
```
现在可以通过数组的方式实现：

```
$user=M('user')->where('id=1');
$user['name']='liming';
$user['year']=23;
$user->save();
```
  
在模板中可以通过“.”操作符直接访问模型数据集。

```
$user=M('user')->where('id=1');
$this->display('user',$user);
```
在user.html模板中可以使用如下方式调用：

```
<p>{$user.id}：{$user.name}-{$user.year}<p>
```
   
### 二、优化M()函数的参数
M函数参数的重新定义如下：

```
/**
 * @param string $name 需要操作的表
 * @param mixed $conn 为字符串时，如果以"^xxx"开头，表示表前缀，否则表示数据库配置名；如果为数组，表示配置
 */
```
可以使用如下几种方式使用M函数：  
  
#### 1. 一般性使用：  
**M(['模型名称'])**

```
$user=M('user');
```
使用默认数据库连接访问user表
  
#### 2. 使用特定数据库连接名称构建模型：  
**M(['模型名称'],['连接名称'])**
例如在配置目录定义有数据库连接：

```
	'sso' => [
		'type'=>  'mysql',     // 数据库类型
		'host'=>  '127.0.0.1', // 服务器地址
		'name'=>  'test',          // 数据库名
		'user'=>  'root',      // 用户名
		'pwd'=>  '',          // 密码
		'port'=>  '3306',        // 端口
		'prefix'=>  's_',    // 数据库表前缀
	],
```
可以使用如下方式访问数据库连接中的表：

```
$user=M('user','sso');
```
#### 3. 动态指定表前缀：   
**M(['模型名称'],['^表名前缀@连接名称'])**  
方式指定重新指定表名前缀

```
//使用u_作为表前缀
$user=M('user','^u_@sso');
```
```
//不使用表前缀
$user=M('user','^@sso');
```
```
//使用默认连接不使用表前缀
$user=M('user','^');
```

#### 4. 增加数据库服务器断开后自动重连
可以在命令行和swoole模式下长时间运行数据库操作程序，如果数据库服务器因为操作时间间隔过长导致自动断开连接，程序自动重新连接数据库服务器，并正常处理数据库操作。

**更多详细使用参见<a target="_blank" href="http://document.thinkphp.cn/manual_3_2.html#define_model">ThinkPHP3.2的模型相关文档</a>**





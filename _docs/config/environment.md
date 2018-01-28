---
title: 环境变量
permalink: /docs/config/environment/
---

为了便于在开发环境和生产环境中使用不同的配置，系统支持配置环境变量。  
配置方式是在系统框架的根目录下建立".env"文件，使用“ini文件”的格式设置。 
  
设置完成之后就可以调用全局函数获取配置：  
**env(['环境变量名称'][,'默认值'])**
   
例如.env文件的配置如下：

```
db_type=mysql
db_host=127.0.0.1
db_name=test
db_user=root
db_pwd=root
db_port=3306
db_prefix=m_
```
在应用的数据库配置文件database.php中使用

```
'default' => [
	'type'=>  env('db_type','mysql'),     // 数据库类型
	'host'=>  env('db_host','127.0.0.1'), // 服务器地址
	'name'=>  env('db_name','test'),          // 数据库名
	'user'=>  env('db_user','root'),      // 用户名
	'pwd'=>  env('db_user','root'),          // 密码
	'port'=>  env('db_port','3306'),        // 端口
	'prefix'=>  env('db_prefix',''),    // 数据库表前缀
],
```

**为了防止本地开发环境的.env文件配置影响线上环境，可以在版本控制工具中忽略".env"文件。**


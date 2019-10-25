---
title: 中间件
permalink: /docs/advance/middleware/
---

### 1. 概念
中间件是在定义在控制器运行之前或之后运行的对象，中间件可以配置一个或多个。
中间件通常用于授权或者处理请求过滤等。

### 2.中间件的定义
路由的中间件在全局配置或应用配置文件中定义，
全局配置位于storage/Conf/middleware.php，应用配置位于app/应用名称/Conf/middleware.php文件中。   
例如以下定义了auth和convert中间件：

```
<?php
return [
	'auth'=>'App\\Home\\Middleware\\Authorize',
	'convert'=>'App\\Home\\Middleware\\CharsetConvert',
];
```

### 3.中间件的开发
如上auth中间件位于命名空间“App\Home\Middleware”中，中间件需要实现handle方法。  
handle方法的参数定义如下：

```
 /**
  * @param \Closure $next
  * @param \Library\Request $request
  * @param \Library\Response $response
  * return \Closure
  */
  function handle(\Closure $next,$request,$response)
```
例如auth中间件定义如下：

```
<?php
namespace App\Home\Middleware;

class Authorize{
	public function handle(\Closure $next,$request,$response){
		return $next($request,$response);
	}
}
```
在handle方法中，需要返回 “$next($request,$response)”用于执行下一个中间件。  
也可以使用下面的方式改变中间件的执行顺序：

```
public function handle(\Closure $next,$request,$response){
	$result=$next($request,$response);
	//处理中间件方法
	return $result;
}
```
这样在控制器执行完成之后才会执行中间件的方法。

### 4. 中间件的路由配置
中间件可以在路由中配置，参考[路由/路由中间件](/docs/route/route_middleware/)。  
也可以在控制器静态方法middleware中返回中间件：

```
<?php
namespace App\Home\Controller;
use Library\Controller;

class Index extends Controller{
	
	public static function middleware(){
		//控制器构造方法中使用中间件
		return 'auth';
	}
	
}
```
以上执行控制器的所有方法前都会先执行中间件，  
可以指定不需要执行的方法：

```
//执行hello方法不会执行中间件
<?php
namespace App\Home\Controller;
use Library\Controller;

class Index extends Controller{
	
	public static function middleware(){
		//控制器构造方法中使用中间件
		return 'auth:hello';
	}
	
	public function hello(){
	}
	
	public function test(){
	}	
	
}
```
也可以使用常量定义中间件
```
//执行hello方法不会执行中间件
<?php
namespace App\Home\Controller;
use Library\Controller;

class Index extends Controller{
	
	const MIDDLEWARE='auth:hello';
	
	public function hello(){
	}
	
	public function test(){
	}	
	
}
```





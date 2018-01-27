---
title: 容器
permalink: /docs/advance/container/
---

### 1. 容器的概述
在Steeze中，容器用于创建和管理类的实例，在容器中创建的类对象都是单例模式。容器根据传入的参数，然后利用反射构建参数对象，然后传入构造函数构建对象。容器同时提供调用对象方法（或Closure）的方式，方法所依赖的参数，容器根据指定的参数类型构建完参数后根据参数名称注入。 

### 2. 容器的使用
在容器使用系统提供的make方法构建对象实例，  
make方法的定义如下：

```
/**
 * 从容器中返回给定类型名称的实例
 *
 * @param string $concrete 类型名称
 * @param array $parameters 参数
 * @return object 类型实例
 */
function make($concrete,array $parameters=[]){
	$container=\Library\Container::getInstance();
	return $container->make($concrete, $parameters);
}
```
例如在系统框架中所使用的Request和Response对象，使用了容器构建：
 
```
<?php
namespace Library;
use Loader;

class Application{
	private $request=null;
	private $response=null;
	
	public function __construct($request=null, $response=null){
		//初始化请求和响应对象
		$this->request=make(Request::class);
		$this->response=make(Response::class);
		
		// ......
		
```



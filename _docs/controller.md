---
title: 控制器
permalink: /docs/controller/
---

## 一、简介
控制器位于每个模块的“Controller”目录下，并继承自“Library\Controller”类型。
控制器的命名空间为“App\模块名称（首字母大写）\Controller“，例如home模块下的控制器命名空间为"App\Home\Controller"。  
一个完整的控制器代码如下所示：

```
<?php
namespace App\Home\Controller;
use Library\Controller;
use Library\Model;
use Library\Request;
use Library\Response;

class Index extends Controller{
	
	private $request;
	private $response;
	
	public function __construct(Request $request,Response $response){
		$this->request=$request;
		$this->response=$response;
	}
	
	/**
	* 对外公开方法，返回字符串
	* @param string $name 为路由参数注入
	* @return 返回字符串到用户浏览器
	*/
	public function hello($name='Guest'){
		return 'Hello！'.$name.'， welcome to visit！';
	}
	
	/**
	* 对外公开方法，返回json
	* @param string $name 可以为路由参数注入
	* @param int $age 可以为路由参数注入
	* @return 返回json到用户浏览器
	*/
	public function info($name='Guest',$age=18){
		return ['name'=>'name','age'=>$age];
	}
	
	/**
	* 对外公开方法，返回渲染后的视图
	* @param Library\Model $user为路由参数传入并系统处理后的用户模型对象
	* @return 返回渲染后的视图对象到浏览器
	*/
	public function profile(Model $user){
		$this->assign('user',$user);
		$this->display();
	}
	
	/**
	* 内部模版标签调用方法，渲染后的子视图嵌入到主视图中
	* @param int $id 由模版标签参数传入
	*/
	public function _show($id){
		$this->assign('id',$id);
		$this->display();
	}
}

```

## 二、方法分类
控制器的方法类型有三种：对外开放方法、内部标签调用方法和其它方法。
### 1. 对外开放方法
对外开放的方法使用“public”声明，方法名称以字母开头。  
在路由控制器中可以配置此类方法的处理，具体配置方式参见[路由配置模块](http://steeze.cn/docs/route/)
### 2. 内部模版标签调用方法
对外开放的方法使用“public”声明，方法名称以下划线“_”开头。  
此类方法用在模版标签中调用，具体调用方式参见[模版视图模块](http://steeze.cn/docs/view/)
### 3. 其它方法
也可以在控制器中使用私有方法或者魔术方法，但通常不建议使用，相关的业务处理可以放在对应的服务类中去完成。

## 三、方法参数分类
控制器的方法参数类型有三种：路由参数、自定义类参数和其它基本类型参数。
### 1. 路由参数
路由参数在路由中配置，然后系统会根据参数名称传入到控制器方法中。  
例如在路由中定义如下路由：

```
'default' => [
	'/show/{catid}/{page}'=> 'Article/show@Home',
]
```
然后在Home模块的Article控制器中增加show方法：

```
<?php
namespace App\Home\Controller;
use Library\Controller;

class Index extends Controller{
	public function show($catid,$page){
		return $catid.'_'.$page;
	}
}

```
在show方法中$catid和$page参数，分别按照名称从路由参数中传入，访问http://127.0.0.1/show/12/1将会输出“12_1”。

***特别提示***：
路由参数只可以应用在对外开放的控制器方法中，不能在内部标签调用方法和私有方法中调用

### 2. 自定义类参数
对于自定义类作为参数，系统容器会在调用控制器方法的时候，根据类型名称实例化参数类型，然后传入执行的控制器方法。
传入的类实例由于是在系统容器中创建，因此类实例将与在其它地方采用容器构建的该类型共享一份实例。
例如在控制器方法中传入Library\Response对象：

```
<?php
namespace App\Home\Controller;
use Library\Controller;
use Library\Response;

class Index extends Controller{
	public function show(Response $response,$catid,$page){
		$response->write($catid.'_'.$page);
	}
}

```
此时访问http://127.0.0.1/show/12/1也会输出“12_1”。

***重要功能提示***：  
对于从模型类Library\Model继承或者是Library\Model自身的类实例，如果满足以下条件：  
1). 参数名称与路由中的参数名称相同；  
2). 在数据库中存在有以参数名称为表名的数据表存在，并且存在字段名称为“id”的主键；  
3). 路由参数传入的是一个数值  
系统会到数据库中查找以参数名称为表名、以参数值为id值那条记录，这样在控制器中就可以直接使用，无需再次进行数据库查询操作。  
例如定义如下路由：

```
'default' => [
	'/profile/{user}'=> 'User/profile@home',
]
```
然后在home模块编写控制器代码：

```
<?php
namespace App\Home\Controller;
use Library\Controller;
use Library\Response;
use Library\Model;

class User extends Controller{
	private $response;
	public function __construct(Response $response){
		$this->response=$response;
	}
	public function profile(Model $user){
		return $user->id.':'.$user->username;
	}
}
```
在上述控制器，访问：http://127.0.0.1/profile/1/，如果在数据库中存在user用户表，将会输出id为1的用户的ID和用户名。

***特别提示***：  
1). 上述功能也可以用在内部模版标签调用方法中，只需要将标签参数当着路由参数传入即可。  
2). 另外需要注意传入的模型对象不在容器中缓存，因此后续新的模型类实例操作对模型参数不会造成影响。


### 3. 其它基本类型参数
其它的基本类型参数对于控制器的对外公开方法意义不大，如果没有设置默认值，将会传入null值。
对于内部标签调用方法，会根据标签参数名称传入。


## 四、返回类型
在控制器方法中，支持直接返回字符串、数组、对象和视图。返回字符串将直接输出到浏览器、返回数组和对象将会转换为json对象输出到浏览器、返回视图对象将会模版渲染后输出。  
  
***特别提示***：  
如果返回的对象是从模型类Library\Model继承或者是Library\Model自身的类实例，返回的json对象是模型最后一次查询后获得的数据集。



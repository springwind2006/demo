---
title: 方法参数类型
permalink: /docs/controller/param_type/
---

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
&nbsp;<a name="model">&nbsp;</a>
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
1). 上述功能也可以用在内部模板标签调用方法中，只需要将标签参数当着路由参数传入即可。  
2). 另外需要注意传入的模型对象不在容器中缓存，因此后续新的模型类实例操作对模型参数不会造成影响。


### 3. 其它基本类型参数
其它的基本类型参数对于控制器的对外公开方法意义不大，如果没有设置默认值，将会传入null值。
对于内部标签调用方法，会根据标签参数名称传入。





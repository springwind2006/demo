---
title: 依赖注入
permalink: /docs/advance/ioc/
---

### 1. 简介

依赖注入的概念源于java的springMVC开发框架，相关概念参见<a href="https://baike.baidu.com/item/%E6%8E%A7%E5%88%B6%E5%8F%8D%E8%BD%AC/1158025?fr=aladdin&fromid=5177233&fromtitle=%E4%BE%9D%E8%B5%96%E6%B3%A8%E5%85%A5" target="_blank">【百度百科】</a>。  
在steeze中，依赖注入提供了更加便捷的实现。在容器中构建对象、或通过容器调用对象方法（或Closure）都采用了依赖注入的。容器会根据类的构造函数所需要的参数类型，构造参数实例，然后传入构造方法完成类实例的构建。  
也可以调用容器提供的方法调用对象方法，从而实现方法的参数的依赖注入。

### 2. 构造方法的注入
下面的示例演示如何使用容器的依赖注入特性自动构造Service服务以及User模型，并在控制器中使用。  
**第一步：创建User模型**

```
<?php 
namespace App\Home\Model;
use Library\Model;

class User extends Model{
	protected $tableName='user';
}

```
**第二步：创建UserService服务**

```
<?php
namespace App\Home\Service;
use App\Home\Model\User;

class UserService{
	private $user=null;
	
	/**
	 * 用户服务构造方法
	 * @param User $user 用户模型对象，由系统框架注入
	 */	
	public function __construct(User $user){
		$this->user=$user;
	}
	
	/**
	 * 根据用户ID获取用户信息
	 * @param int $id 用户ID值
	 */
	public function getUserById($id){
		$where['id']=$id;
		return $this->user->where($where)->find();
	}
}
```

**第三步：在控制器中使用UserService服务**

```
<?php
namespace App\Home\Controller;
use Library\Controller;
use App\Home\Service\UserService;

class User extends Controller{
	private $userService; //用户服务对象
	
	/**
	 * 控制器构造方法
	 * @param UserService $userService 用户服务对象，由系统框架注入
	 */
	public function __construct(UserService $userService){
		$this->userService=$userService;
	}
	
	/**
	 * 获取用户方法
	 * @param int $id 用户ID
	 */
	public function info(int $id){
		$user=$this->userService->getUserById($id);
		$this->assign('user',$user);
		$this->display();
	}
}
```
在User控制器的构造方法中，用户服务UserService对象由系统负责构建，并在构造用户服务的时候，自动注入User模型对象到用户服务UserService类的构造方法中。

### 3. 一般方法的注入调用
控制器的对外方法，由系统框架调用容器的callMethod调用，从而实现控制器方法参数的依赖注入。  
callMethod方法定义如下：  

```
/**
 * 对象方法调用
 * 
 * @param object|string $concrete 实例或类名
 * @param string $method 方法名称
 * @param array $parameters 方法参数
 * @return mixed
 */

public function callMethod($concrete,$method,array $parameters=[])
```
调用位置在Library/Controller类的run静态方法中调用，  
部分代码如下：

```
$container=Container::getInstance();
$result=$container->callMethod($concrete, $method, $parameters);
```
   
也可以调用自定义类的方法，如在以上UserService类中增加获取用户信息的方法：

```
/**
 * 根据用户ID获取用户信息
 * @param int $id 用户ID值
 */
public function getUser(User $user){
	return $user;
}
```
然后通过容器调用getUser方法：

```
$container=Container::getInstance();
$parameters=[
	'user'=>1   //控制器方法参数，此处名称需要和控制器的方法参数的名称一致
];
$result=$container->callMethod('App\\Home\\Service\\UserService', 'getUser', $parameters);
```
关于更多自动注入模型对象的介绍，参见[控制器/方法参数类型](/docs/controller/param_type/#model)

### 4. Closure方法的调用
Closure方法即匿名函数，Steeze支持Closure方法的依赖注入。调用容器的callClosure方法从而实现Closure方法的参数自动注入。  
callClosure方法的定义如下：  

```
/**
 * Closure匿名函数调用
 *
 * @param Closure $closure 匿名函数对象
 * @param array $parameters 方法参数
 * @return mixed
 */

public function callClosure($closure,array $parameters=[])
```
调用实例如下所示：

```
echo Container::getInstance()->callMethod(function(User $user){
	$name=$user->username;
	return 'steeze_'.$name;
}, ['user'=>1]);
```
上述方法中输出ID为1用户的用户名，并在输出的名称前加上"steeze_"前缀。



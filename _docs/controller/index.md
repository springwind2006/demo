---
title: 控制器简介
permalink: /docs/controller/index/
---

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
	* 内部模板标签调用方法，渲染后的子视图嵌入到主视图中
	* @param int $id 由模板标签参数传入
	*/
	public function _show($id){
		$this->assign('id',$id);
		$this->display();
	}
}

```


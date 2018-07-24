---
title: 控制器分组
permalink: /docs/controller/group/
---


Steeze框架支持控制器的无限层级分组，利用分组可以将多个功能相似的控制器放到同一个组内，从而实现应用的模块化部署。  
    
### 1. 定义控制器分组
一个简单的控制器分组如下所示：

```
├── Controller            应用的控制器目录
│   ├── Index.php         控制器类
│   └── Member            控制器分组名称
│       └── Index.php     控制器类
└── View                     视图目录  
    └── Default              主题
        ├── Index            控制器名称
        │   ├── hello.html   视图模板
        │   ├── show.html    同上
        │   └── test.html    同上
        ├── Member           分组名称
            └── Index        控制器名称
                ├── hello.html  视图名称
                └── info.html   同上
```

在分组Member中定义Index控制器：  

```
<?php
//注意需要将分组名称加入命名空间中
namespace App\Home\Controller\Member;
use Library\Controller;

class Index extends Controller{
	// 定义控制器方法
	public function hello(){
		$this->display();
	}
}
```
在hello方法中调用display方法会自动定位到分组下的模板。
     
### 2. 控制器分组的视图路径
视图默认的路径为：  
  
**视图目录[/主题][/分组1][/分组2][...][/控制器名称][/视图名称]**

调用当前分组中的其它控制器视图：  
**display('[控制器名称]/[视图名称]')**    

例如在Index控制器的hello方法中调用同组的User控制器下的index视图

```
	$this->display('User/index');
```
   
  
在当前控制器中调用最顶层的控制器视图使用：  
**display('/[控制器名称]/[视图名称]')**     
  
例如在Index控制器的hello方法中调用顶层Index控制器下的show视图：

```
	$this->display('/Index/show');
```
   
在当前控制器中调用最顶层的分组下控制器视图使用：  
**display('/[分组名称]/[控制器名称]/[视图名称]')**     
  
例如在Index控制器的hello方法中，调用顶层User分组内Index控制器下的show视图：

```
    $this->display('/User/Index/show');
```
  
在当前控制器中调用上级分组下控制器视图使用：  
**display('../[控制器名称]/[视图名称]')**     
  
例如在Admin分组内Index控制器的hello方法中，调用跟Admin分组同级的User分组中的Index控制器下show视图：

```
    $this->display('../User/Index/show');
```
  
---
**以“/”开头的视图调用，从视图根目录向下查找视图**  
**以“../”开头的视图调用，从当前分组向上查找（级数由“../”个数决定），然后向下查找视图**  
**不以“/”或“../”开头的视图调用，从当前分组向下查找视图**




  







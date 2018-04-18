---
title: 模板标签
permalink: /docs/view/tpl_tags/
---

## 一、变量赋值
在模板中使用assign标签为变量赋值，赋值之后就可以在后续的模板中使用。 
  
**assign标签使用格式：**  
&lt;assign name="变量名称" value="变量值" /&gt; 
   
使用范例：

```
<assign name="year" value="13" />
```
在后续的模板中，可以用表达式输出：

```
{$year}
```
如果在模板中已经有相同名称的变量，会为变量重新赋值。

## 二、条件判断
用法示例：

```
<if condition="($name eq 1) OR ($name gt 100) "> value1
<elseif condition="$name eq 2"/>value2
<else /> value3
</if>
```
在condition属性中可以支持eq等判断表达式，同上面的比较标签，但是不支持带有”>”、”<”等符号的用法，因为会混淆模板解析，所以下面的用法是错误的：

```
<if condition="$id < 5 ">value1
    <else /> value2
</if>
```
必须改成：

```
<if condition="$id lt 5 ">value1
<else /> value2
</if>
```

系统支持的比较标签以及所表示的含义分别是：

<table>
	<tr>
		<th>标签</th>
		<th>	含义</th>
	</tr>
	<tr>
		<td>eq</td>
		<td>	等于</td>
	</tr>
	<tr>
		<td>neq</td>
		<td>不等于</td>
	</tr>
	<tr>
		<td>gt</td>
		<td>	大于</td>
	</tr>
	<tr>
		<td>egt</td>
		<td>	大于等于</td>
	</tr>
	<tr>
		<td>lt</td>
		<td>小于</td>
	</tr>
	<tr>
		<td>elt</td>
		<td>	小于等于</td>
	</tr>
	<tr>
		<td>heq</td>
		<td>恒等于</td>
	</tr>
	<tr>
		<td>nheq</td>
		<td>不恒等于</td>
	</tr>
</table>


除此之外，我们可以在condition属性里面使用php代码，例如：

```
<if condition="strtoupper($user['name']) neq 'Steeze'">Steeze
<else /> other Framework
</if>
```

condition属性可以支持点语法，用于访问数组元素

```
<if condition="$user.name neq 'Steeze'">Steeze
<else /> other Framework
</if>
```


## 三、循环输出

### 1. for标签
用法说明：

```
<for start="开始值" end="结束值" comparison="" step="步进值" name="循环变量名" >
</for>
```
开始值、结束值、步进值和循环变量都可以支持变量，开始值和结束值是必须，其他是可选。comparison 的默认值是lt;；name的默认值是i，步进值的默认值是1，举例如下：

```
<for start="1" end="100">
{$i}
</for>
```

解析后的代码是：

```
for ($i=1;$i<100;$i+=1){
    echo $i;
} 
```

### 2. foreach标签
foreach标签的别名是loop，用法如下：

```
<foreach name="数组变量名称" item="数组的值名称" key="数组的键名称" >
</foreach>
```

foreach标签通常用于查询数据集（select方法）的结果输出，通常模型的select方法返回的结果是一个二维数组，可以直接使用foreach标签进行输出。在控制器中首先对模板赋值：

```
$User = M('User');
$list = $User->limit(10)->select();
$this->assign('list',$list);
```
在模板定义如下，循环输出用户的编号和姓名：

```
<foreach name="list" item="user">
{$user.id}:{$user.name}<br/>
</foreach>
```
解析后的代码是：

```
<?php foreach($list as $user){ ?>
<?php echo $user['id'];?>:<?php echo $user['name'];?><br/>
<?php } ?>
```
如果需要输出键值，可以使用key属性：

```
<foreach name="list" item="user" key="index">
{$index}=>{$user.id}:{$user.name}<br/>
</foreach>
```
解析后的代码是：

```
<?php foreach($list as $index=> $user){ ?>
<?php echo $index;?>=><?php echo $user['id'];?>:<?php echo $user['name'];?><br/>
<?php } ?>
```


## 四、子模板包含
子模板包含使用include（别名是template）标签，用法是：

```
<include file='模板表达式或者模板文件1,模板表达式或者模板文件2,...' />
```
### 1. 使用模板表达式
模板表达式的定义与display方法的第一个参数规则一致：   
**控制器分组/控制器/操作@主题:应用名**  
   
如下所示：  

```
<include file="Index/show" /> // 包含Index控制器的show方法指向的模板
<include file="User/Index/info" /> // 包含User控制器分组下Index控制器的info方法指向的模板
<include file="Index/show@New" /> // 包含New主题下面Index控制器的show方法指向的模板
```
也可以一次性传入多个模板：

```
<include file="Index/show,User/Index/info,Index/show@New" /> // 同时包含多个模板
```

**对于多层级控制器分组下的模板包含：**   
如果表达式以“/”开头的视图调用，从视图根目录向下查找视图；  
如果表达式不以“/”开头的视图调用，从当前分组向上查找视图。
  
例如，在Member控制器分组下分别存在Index和User的控制器对应的视图模板文件：

```
├── Product
│   ├── show.html
│   └── test.html
├── Member
│   ├── Index
│   │   ├── hello.html
│   │   └── info.html
│   └── User
│       ├── hello.html
│       └── info.html
│
```
如果当前的URL对应的控制器为Member分组下的Index，
那么在Index的hello方法对应的模板中，包含User控制器的hello方法对应的模板，采用如下方式：

```
<include file="User/hello" />
```
系统会根据当前的分组名称自动定位同一个分组下的控制器模板。  
   
如果需要访问顶层控制器Product控制器的show方法对应的模板，采用如下方式：

```
<include file="/Product/show" />
```
同样，如果在Product控制器的show方法包含Index控制器hello方法对应的模板，可以采用：

```
<include file="/Member/Index/hello" />
```
或者

```
<include file="Member/Index/hello" />
```
两种方式都可以。


### 2. 使用模版文件
可以直接包含一个模版文件名（包含完整路径），例如：

```
<include file="./app/home/View/default/Index/user.html" />
```

## 五、布局
Steeze的模版布局是为了弥补模板包含功能的缺陷。多个模版使用同一个布局，只要修改其中一部分代码即可，无需多次包含。  
首先在模版中使用layout标签声明使用的布局：

```
<layout name="布局表达式"/>
```
布局表达式的与include标签的模板表达式语法一样。  
然后使用slice标签定义布局代码片断：

```
<slice name="代码片断名称">
	代码片断内容
</slice>
```
最后在布局文件中引用代码片断：

```
<slice name="代码片断名称" />
```
&nbsp;  
例如以下Layout目录下存在base.html布局文件：

```
├── Index
│   └── test.html
├── Layout
│   └── base.html
```
现在需要在Index/test.html中使用布局文件，Index/test.html的文件内容如下：

```
<layout name="Layout/base"/>
<assign name="title" value="我是Test文件标题" />
<slice name="body">
	我是Test文件内容
</slice>
```
布局文件Layout/base.html的内容如下：

```
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<title>{$title}</title>
</head>
<body>
<slice name="body"/>
</body>
</html>
```
模版文件Index/test.html经过渲染后的结果是：

```
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<title>我是Test文件标题</title>
</head>
<body>
	我是Test文件内容
</body>
</html>
```
   
**特别提示：**
1. 由于系统对模板的缓存机制，如果对预先定义好的视图布局文件再次进行修改。新的布局不会立即生效，需要清空模版缓存后才生效（如果确实需要，可以定义TEMPLATE_REPARSE常量为true，则系统不使用模版缓存）。  
2. 在使用布局的模版文件中，如果assgin标签在slice标签外部，会作为全局模版变量，布局文件或slice内部定义的同名模版变量会覆盖同名全局模版变量。

## 六、调用控制器标签方法
使用action标签调用控制器的标签方法，极大地方便了应用中模块化开发。 
   
### 1. action标签的用法 
&lt;action name="调用方法表达式" app="调用的应用名称" return="返回值变量名" attr1="其它属性值1" attr2="其它属性值2"  ... /&gt;  
  
### 2. action标签属性 
除了name属性为必需，其它属性为可选。如果app属性未指定，则默认使用当前应用；return属性用于指定返回值的变量名称，如果不指定，但调用的方法具有返回值，则会在模板的调用位置输出；其它属性值会按照属性名称传递到调用的控制器方法中。

### 3. 控制器的标签方法定义  
控制器的标签方法采用下划线“\_”开头，方法参数为标签定义的属性。  
在标签方法可以有返回值，返回值可以在模板的其它地方调用。  
也可以使用控制器的display方输出标签方法对应的模板文件（不带开头下划线“\_”）。
  
#### 1). 有返回值的标签方法   
在模版中使用action标签调用User控制器的info标签方法。  
首先在User控制器中定义info标签方法：

```
namespace App\Home\Controller;
use Library\Controller;
use Library\Model;

class Index extends Controller{
	/**
	* 获取用户信息的标签方法
	* @param mixed $id 用户ID，标签参数传入
	* @return array 用户信息
	* 说明：标签方法名称以下划线“_”开头，并声明为public
	*/
	public function _info($id=0){
		$user=M('User')->where('id='.intval($id))->find();
		return $user;
	}
}
```
然后在模板中调用标签方法：

```
<action name="User/info" id="13" return="user"/>
<p>用户ID：{$user.id}，用户名：{$user.name}</p>
```

#### 2). 在标签方法使用display方法

当没有返回值时，可以在标签方法中调用控制器的display方法渲染标签方法对应的模板：

```
public function _info($id=0){
	$user=M('User')->where('id='.intval($id))->find();
	$this->assign('user',$user);
	$this->dislay();
}
```
标签方法对应的模板文件info.html内容如下：

```
<p>用户ID：{$user.id}，用户名：{$user.name}</p>
```

此时在模版中调用：

```
<h3>查看用户信息：</h3>
<action name="User/info" id="13"/>
```
模版渲染后的结果为：

```
<h3>查看用户信息：</h3>
<p>用户ID：12，用户名：liming</p>
```
  
另外标签方法也支持类似路由参数的模型注入，详细用法参见[控制器/方法参数类型](/docs/controller/param_type/#model)
  
## 七、静态资源文件

Steeze提供了强大的import标签用于静态文件的引入，资源文件包含css文件、js文件、图片文件，以及其它类型的文件。

### 1、import标签的用法
&lt;import file="文件名称" type="类型" check="是否检查文件存在" default="默认主题包名" base="相对路径" attr1="其它属性值1" attr2="其它属性值1"  ... /&gt;  

### 2、标签属性
import标签的主要属性如下：
<table>
	<tr>
	  <th>属性名称</th>
	  <th>属性值</th>
	  <th>是否可选</th>
	  <th>说明</th>
	</tr>
	<tr>
	  <td>file</td>
	  <td>文件路径</td>
	  <td>必须</td>
	  <td>可以使用相对路径，如果使用“/”开头，相对于系统资源目录"/assets"</td>
	</tr>
	<tr>
	  <td>type</td>
	  <td>js、css，或jpg、png...</td>
	  <td>可选</td>
	  <td>如果文件路径不是以扩展名结尾，需要指明文件类型</td>
	</tr>
	<tr>
	  <td>check</td>
	  <td>true或false</td>
	  <td>可选</td>
	  <td>默认：false，如果为true，则检查文件是否存在，不存在则不输出</td>
	</tr>
	<tr>
	  <td>default</td>
	  <td>默认主题包</td>
	  <td>可选</td>
	  <td>当check属性为true，并且文件不存在时，默认使用的主题包名</td>
	</tr>
	<tr>
	  <td>base</td>
	  <td>基础相对路径</td>
	  <td>可选</td>
	  <td>一般用于当file为多个以“,”分割的路径时使用的共同相对路径</td>
	</tr>
	<tr>
	  <td>其它属性名</td>
	  <td>标签的其它属性值</td>
	  <td>可选</td>
	  <td>例如文件类型为图片时，可以指明文件大小，使用：width="12px"</td>
	</tr>
</table>



**一般用法**  
例如在模板中引入index.css文件：

```
 <import file="index.css"/>
```
生成的标签代码为：

```
<link rel="stylesheet" type="text/css" href="/assets/app/home/default/css/index.css" />
```
   
### 3. 资源文件路径
**所有静态资源文件统一放在public下的assets子目录中**   
  
多应用部署模式下，浏览器访问的资源文件路径为：   
/assets/app/[应用名称]/[主题包名]/[文件分类]/[资源文件名称] 
   
单应用的情况下，也支持：  
/assets/[文件分类]/[资源文件名称]  

如果file属性以“/”开头，则文件路径相对性于/assets路径，例如：

  
```
 <import file="/user.css"/>
```
生成的标签代码为：

```
<link rel="stylesheet" type="text/css" href="/assets/user.css" />
```
   
资源文件的路径可以由system配置文件中default_assets键配置。  
如default_assets配置是其它网站的URL路径：

```
'default_assets' => 'http://www.steeze.com/assets', // 前台访问静态文件路径
```
模板标签：

```
 <import file="index.css"/>
```
生成的前端代码为：

```
<link rel="stylesheet" type="text/css" href="http://www.steeze.com/assets/css/index.css" />
```
如果default_assets配置以"/"开头：

```
'default_assets' => '/default', // 前台访问静态文件路径
```

生成的前端代码为：

```
<link rel="stylesheet" type="text/css" href="/assets/default/css/index.css" />
```

如果default_assets配置不以"/"开头，则为风格包名：

```
'default_assets' => 'new_style', // 风格包名
```

生成的前端代码为：

```
<link rel="stylesheet" type="text/css" href="/assets/app/home/new_style/css/index.css" />
```


### 4. 分类名称的自动推导

上述范例中“css/"的路径，是系统根据文件名称推断出文件类型，从而自动生成的。  
自动类型推导仅支持css、js和图片文件类型（jpg、jpeg、png、gif、bmp）。  
自动推导的后的文件路径，会在现有的路径前加上该类型资源所在的目录（相对于静态资源目录）：  
css文件对应的路径为“css/”，js文件为“js/”，图片文件为“images/”。
    
如果file属性值不是以扩展名结尾，系统无法根据文件名称推导类型，需要使用type属性指定：

```
 <import file="index.css?version=1.0" type="css"/>
```
生成的前端代码为：

```
<link rel="stylesheet" type="text/css" href="/assets/app/home/default/css/index.css?version=1.0" />
```
  
如果包含base属性（或以该资源类型目录开头），系统则不会根据文件类型自动推导分类名称，例如：
  
```
 <import file="user.css" base="index"/>
 <import file="css/info.css" />
```
生成的标签代码为：

```
<link rel="stylesheet" type="text/css" href="/assets/app/home/default/index/user.css" />
<link rel="stylesheet" type="text/css" href="/assets/app/home/default/css/info.css" />
```
   
以非资源类型目录开头（不包括“/”），系统可以完成推导：

```
 <import file="index/user.css" />
```
生成的前端代码为：

```
<link rel="stylesheet" type="text/css" href="/assets/app/home/default/css/index/user.css" />
```


### 5. 引入多个资源文件
引入多个资源文件，file属性的值可以使用半角逗号“,”分割，例如：

```
 <import file="index.css,user.css,index.js"/>
```
上述模板标签生成的代码为：

```
<link rel="stylesheet" type="text/css" href="/assets/app/home/default/css/index.css" />
<link rel="stylesheet" type="text/css" href="/assets/app/home/default/css/user.css" />
<script type="text/javascript" src="/assets/app/home/default/js/index.js" ></script>
```
   
可以使用base属性对多个文件进行分组

```
 <import file="index.css,user.css,index.js" base="common/lib"/>
```
生成的代码为：

```
<link rel="stylesheet" type="text/css" href="/assets/app/home/default/common/lib/index.css" />
<link rel="stylesheet" type="text/css" href="/assets/app/home/default/common/lib/user.css" />
<script type="text/javascript" src="/assets/app/home/default/common/lib/index.js" ></script>
```
**注意：使用了base属性之后系统根据文件扩展名自动推导文件类型名称**

### 6. 检查资源文件是否存在
可以使用check属性对引入资源文件进行检查，如果不存在则不输出。  
例如如下代码

```
 <import file="index.css" check="true"/>
```
如果“/assets/app/home/default/css/index.css”文件不存在，则不输出。  
   
配合default属性，可以指定当前主题包下的文件不存在时，使用指定的默认主题包下的文件。  

```
 <import file="index.css" check="true" default="newstyle"/>
```
如果当前的主题包“default”下文件“/assets/app/home/default/css/index.css”不存在时。  
使用指定的默认主题“newstyle”，因此以上模板输出：

```
<link rel="stylesheet" type="text/css" href="/assets/app/home/newstyle/css/index.css" />
```
但如果“/assets/app/home/newstyle/css/index.css”也不存在，则不输出。

### 7. 使用其它属性
import标签支持除了默认属性外的其它属性，其它属性会原样输出到生成的标签文件。  
例如以下模板引入图片文件：

```
<import file="/a.jpg" style="border:none" />
```
生成的HTML代码为：

```
<img src="/assets/a.jpg" style="border:none"/>
```

## 八、使用php代码
php代码可以和标签在模板文件中混合使用，可以在模板文件里面书写任意的PHP语句代码 ，包括下面两种方式：
### 1. 使用php标签
例如：

```
<php>
	echo 'Hello,world!';
	echo $user['name'];
</php>
```
模板引擎会将&lt;php&gt;和&lt;/php&gt;标签转换为对应的“&lt;?php”和“?&gt;”，上述代码输出结果为：

```
<?php
	echo 'Hello,world!';
	echo $user['name'];
?>
```

### 2. 使用原生php代码
steeze同时支持php原生代码，例如：

```
<?php
	echo 'Hello,world!';
	echo $user['name'];
?>
```
&nbsp;<a name="extend"></a>
**注意**：以上使用php代码的两种方式，代码中不能使用点语法变量输出（例如：{$user.name}），同时也不能使用任何的模标签和语法，否则会出现模板解析错误。

## 九、调用扩展标签库
steeze模板中同时支持调用扩展的标签库，调用方式类似于Java的Struts中的JSP标签库。
    
**非闭合标签的调用语法：**   
 
```
<标签库名称:标签方法 属性名称1="属性值1"  属性名称2="属性值2" ... />
```
  
**闭合标签的调用语法：**    

```
<标签库名称:标签方法 属性名称1="属性值1"  属性名称2="属性值2" ... > 
  标签中的内容  
</标签库名称:标签方法>
```

扩展标签库分为系统扩展标签库和用户自定义标签库：系统的扩展标签库位于/kernel/Service/Template/Taglib目录下；用户自定义的标签库位于应用的Taglib目录下。  
   
调用系统扩展标签库St的json方法（此方法为非闭合标签方法），模板代码如下：

```
<st:json url="http://test.com/search.json" return="datas"/>
<foreach name="datas" item="item">
	<p>{$item.title}:{$item.url}</p>
</foreach>
```
上述模板标签直接从网络上获取json数据，并在页面中输出。
   
调用系统扩展标签库St的get方法（此方法为闭合标签方法），模板代码如下：

```
<st:get table="news" field="id,title" return="newses" order="id asc" page="$page">
	<table cellpadding="0" cellspacing="0">
		<foreach name="newses" item="item">
			<tr><td>{$item.id}</td><td>{$item.title}</td></tr>
		</foreach>
	</table>
</st:get>
<p>{$pages}</p>
```
上述模板标签直接查询news数据表，并输出ID、标题以及分页信息。


**说明：用户自定义标签库名称和系统扩展标签库名称冲突时，系统优先使用用户自定义标签库**  
  
关于用户自定义标签库的开发，详见[视图/标签扩展](/docs/view/custom_tags/)。











---
title: 模板输出
permalink: /docs/view/tpl_output/
---

### 1. 变量的输出
在模板中输出变量的方法很简单，例如，在控制器中我们给模板变量赋值：

```
$name = 'World';
$this->assign('name',$name);
$this->display();
```
然后就可以在模板中使用：

```
Hello,{$name}！
```
模板编译后的结果是：

```
Hello,<?php echo($name);?>！
```
这样，运行的时候就会在模板中显示： Hello,World！
  
注意模板标签的{和$之间不能有任何的空格或其它字符，否则标签无效。所以，下面的标签

```
Hello,{ $name}！
```
将不会正常输出name变量，而是直接保持不变输出： Hello,{ $name}！
  
如果变量的类型是数组，可以用"."输出

```
$data['name'] = 'liming';
$data['email'] = 'liming@163.com';
$data['company'] = array(
	'name'=>'St.ltd',
	'address'=>'Beijing'
);
$this->assign('data',$data);
```
那么，在模板中我们可以用下面的方式输出：

```
Name：{$data.name}
Email：{$data.email}
```
对于多维数组，我们也采用同样的方式输出：

```
Company Name：{$data.company.name}
Company Address：{$data.company.email}
```
如果data是一个对象，采用如下方式输出：

```
Name：{$data->name}
Email：{$data->email}
```


输出系统全局变量$_SERVER、$_ENV、 $_POST、 $_GET、 $_REQUEST、$_SESSION和 $_COOKIE，  
也采用相同的方式

```
//输出$_SERVER['SCRIPT_NAME']变量
{$_SERVER.script_name}
//输出$_SESSION['user_id']变量
{$_SESSION.user_id} 
//输出$_GET['pageNumber']变量
{$_GET.pageNumber}
//输出$_COOKIE['name']变量
{$_COOKIE.name}
```
**对于全局变量，如果在模板中赋值相同的名称，会覆盖系统全局变量**

### 2. 常量的输出
输出系统中定义的常量方式：

```
 //应用所在路径
{APP_PATH} 
 //默认应用名称
{DEFAULT_APP_NAME}
```
此种方式也支持直接输出系统中定义的环境变量，但常量的优先级别高于环境变量

### 3. 使用函数
我们往往需要对模板输出变量使用函数，可以使用：

```
{$data.name|md5} 
```
编译后的结果是：

```
<?php echo (md5($data['name'])); ?>
```

如果函数有多个参数需要调用，则使用：

```
{$create_time|date="y-m-d",###}
```
表示date函数传入两个参数，每个参数用逗号分割，这里第一个参数是y-m-d，第二个参数是前面要输出的create_time变量，因为该变量是第二个参数，因此需要用###标识变量位置，编译后的结果是：

```
<?php echo (date("y-m-d",$create_time)); ?>
```

如果前面输出的变量在后面定义的函数的第一个参数，则可以直接使用：

```
{$data.name|substr=0,3}
```
表示输出

```
<?php echo (substr($data['name'],0,3)); ?>
```
虽然也可以使用：

```
{$data.name|substr=###,0,3}
```
但完全没用这个必要。

还可以支持多个函数过滤，多个函数之间用“\|”分割即可，例如：

```
{$name|md5|strtoupper|substr=0,3}
```
编译后的结果是：

```
<?php echo (substr(strtoupper(md5($name)),0,3)); ?>
```
函数会按照从左到右的顺序依次调用。

如果你觉得这样写起来比较麻烦，也可以直接这样写：

```
{substr(strtoupper(md5($name)),0,3)}
```

### 4. 表达式的输出
你可以使用这样的方式输出表达式的值：{(表达式)}   
例如输出三目运算的表达式： 
 
```
{($year==18?'少年':'青年')}
{($year+23)}
{($year*$year)}
```
注意的是"{"和"("、")"和"}"之间不能有空格，“{(”与")}"之间可以是任意有输出值的表达式。  
上面的模板编译后的结果是：

```
<?php echo ($year==18?'少年':'青年'); ?>
<?php echo ($year+23); ?>
<?php echo ($year*$year); ?>
```
  
在表达式中也可以使用“.”选择数组元素：

```
{($company.address=='Beijing'?'北京地区':'其它地区')}
```
模版引擎会自动编译为：

```
<?php echo ($company['address']=='Beijing'?'北京地区':'其它地区'); ?>
```

### 5. 使用运算符
目前支持自增和自减运算符  
如下所示：

```
{++$year}
{$year++}
{$year--}
{--$year}
```
编译后的结果为：

```
<?php ++$year;?>
<?php $year++;?>
<?php $year--;?>
<?php --$year;?>
```

### 6. 其它输出
支持动态输出资源文件的URL地址  
如下所示：

```
{@index.js}  //使用自动扩展名解析
{@#test.js}  //不适用自动扩展名解析
```
编译后的结果为：

```
<?php asset('index.js');?> //输出：http://www.steeze.cn/assets/js/index.js
<?php asset('#test.js');?> //输出：http://www.steeze.cn/assets/test.js
```



---
title: 标签扩展
permalink: /docs/view/custom_tags/
---

Steeze支持用户自定义标签，扩展现有的标签库，从而满足不同的业务需求。  
扩展标签库的使用参见[模板标签/调用扩展标签库](/docs/view/tpl_tags/#extend)
  
**标签库的位置**：  
应用根文件夹/应用目录/Taglib/标签库文件
  
**标签库文件模板**:

```
namespace App\应用名\Taglib;

class 标签库名{
	public function parse标签方法名(标签属性,标签文本){
		标签处理过程
		...
		return 返回到模板的内容；
	}
}
```
**标签方法的命名方式是：parse+标签方法名（首字母大写）**。  
**应用名和标签库名遵循统一的命名规范，都是首字母大写**。  
       
下面的例子演示在home应用下开发自定义标签库“ht"的流程：  
在home应用的根目录下建立标签库文件夹“Taglib”，然后在Taglib目录下建立Ht.php文件。  
文件目录结构如下：

```
├── app
│   └── home
│         └── Taglib
│                └── Ht.php
│
```

Ht.php文件的内容为：

```
<?php
namespace App\Home\Taglib;

class Ht{
	/**
	 * md5标签
	 * @param array $attrs 标签属性
	 * @param string $html 标签HTML内容，是在闭合标签的时候传入，非闭合标签为空
	 * @return string 标签输出文本
	 * */
	public function parseMd5($attrs,$html=''){
		$str=isset($attrs['str'])?$attrs['str']:'';
		return '<?php echo md5(\'' . $str . '\') ?>';
	}
}
```
在模板中调用标签：

```
“hello”的MD5值：
<ht:md5 str="hello"/>
```
模版渲染后的结果为：

```
“hello”的MD5值：
5d41402abc4b2a76b9719d911017c592
```

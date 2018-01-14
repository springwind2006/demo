---
title: 动态配置
permalink: /docs/config/set_config/
---

在系统运行时期，可以动态设置配置：

```
<?php
C([
	'lang' => 'zh-cn',
	'debug' => 0, 
	'lock_ex' => 1,
],'test');
```
如果第二个参数不提供，默认为system（系统配置）。

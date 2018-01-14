---
title: 方法返回类型
permalink: /docs/controller/return_type/
---


在控制器方法中，支持直接返回字符串、数组、对象和视图。返回字符串将直接输出到浏览器、返回数组和对象将会转换为json对象输出到浏览器、返回视图对象将会模板渲染后输出。  
  
***特别提示***：  
如果返回的对象是从模型类Library\Model继承或者是Library\Model自身的类实例，返回的json对象是模型最后一次查询后获得的数据集。

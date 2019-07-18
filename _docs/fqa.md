---
title: 常见问题
permalink: /docs/fqa/
---

#### 1. Steeze框架是免费的吗？
Steeze框架有个人开发，是完全免费的，您可以无限制的使用和修改框架的代码，[官方github](https://github.com/steezer/steeze)会不定期发布最新的升级版本。
  
#### 2. Steeze和ThinkPHP框架相比有什么特别的地方？
Steeze的Model层使用ThinkPHP实现，同时优化了部分代码，简化了路由配置，增加了中间件、容器和依赖注入功能。同时支持控制器分组功能。
  
#### 3. Steeze与Laravel框架相比有什么特点？
Steeze是轻量级的框架，吸取了Laravel框架的中间件、容器和依赖注入的特性，同时支持控制器分组、优化了路由配置。
  
#### 4. Steeze可以用于企业级开发吗？
Steeze完全支持企业级开发的特性，可以使用模块化部署项目，支持控制器无限级功能分组，利用中间件和容器的特性，能够减少模块之前的依赖，自定义视图引擎，能够有效实现前后端分离。
  
#### 5. Steeze支持大流量高并发的开发吗？
Steeze全面支持在swoole下运行，swoole改变了PHP的运行模式，不再是像php-fpm那样采用多进程，而是使用多线程，同时支持异步编程。在同样的代码下，swoole模式下性能是php-fpm常规模式的近10倍。



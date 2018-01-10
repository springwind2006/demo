---
title: 目录结构
permalink: /docs/startup/directory_structure/
---

下载解压到之后，可以看到初始的系统目录（部分目录需要在运行时自动生成）

```
├── LICENSE         LICENSE文件    
├── README.md       说明文件
├── app             应用目录
│   ├── console     控制台命令行工具
│   └── home        默认Web应用（应用范例文件）
├── kernel          系统内核框架
│   ├── Helper      系统函数
│   ├── Lang        语言包
│   ├── Library     系统类库
│   ├── Service     系统服务
│   ├── View        系统视图文件
│   └── base.php    引导文件
├── public          Web根目录
│   ├── assets      资源目录，包含js、css、images等静态资源
│   ├── favicon.ico ICO文件
│   └── index.php   系统入口文件
├── storage         数据存储目录（需要读写权限）
│   ├── Conf        系统配置目录
│   ├── Data        系统数据        
│   ├── Cache       缓存目录（在系统首次运行后自动生成）
│   ├── Logs        日志文件（在系统首次运行后自动生成）
│   ├── Routes      分布式路由配置（如果需要的话可以配置）
│   └── .....       其它配置
└── tests
```
另外每个应用都有独立的目录结构，以home应用为例：

```
├── app    应用根目录
│   └── home             应用名称 
│       ├── Conf         应用配置
│       ├── Controller   控制器
│       ├── Middleware   中间件
│       ├── Model        模型
│       └── View         视图
│           └── Default  风格包名
```

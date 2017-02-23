---
title: 软件架构—MVC,MVP,MVVM
tags: 软件架构
toc: true
---

## 背景
主要探讨当前热门的**MVC**,**MVP**,**MVVM**的含义，主要参考[阮一峰的MVC，MVP 和 MVVM 的图示](http://www.ruanyifeng.com/blog/2015/02/mvcmvp_mvvm.html)。

## MVC
MVC，Model-View-Controller。
- View，视图层，主要指用户界面
- Model，数据模型层，指的是交互的数据模型
- Controller，控制层，主要是接受View层的请求，处理执行逻辑，得到数据模型Model，讲数据模型传递给View层。
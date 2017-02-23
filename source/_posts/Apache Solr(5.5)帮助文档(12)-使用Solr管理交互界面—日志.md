---
title : Apache Solr(5.5)帮助文档(12)-使用Solr管理交互界面—日志
tags : Apache Solr(5.5)翻译
toc : true
---

>Logging

日志页面展示了Solr日志文件中的信息。
当点击"Logging"链接，可以看到一张与下图类似的界面


![日志界面，包含了由于客户端传递错误document而产生的错误日志](http://upload-images.jianshu.io/upload_images/1213316-b083ee95db102e88.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上述例子只是展示了一个core的日志。如果单个实例中有多个Core，每一个都会被展示出来，并有对应的日志级别。

### 选取一个日志级别

![日志级别](http://upload-images.jianshu.io/upload_images/1213316-18d866086fb9a4c5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

当点击了左侧**Level**链接，可以看到实例中的classpath和classname的层级。黄色高亮表明该类具有日志能力。点击一个高亮的行，会弹出一个菜单，通过该菜单可以修改日志级别。黑色字段表明该类不会因为root的日志级别改变而改变自己的日志级别。
更多细节可以查看**Configuring Logging**章节。
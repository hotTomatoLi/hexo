---
title: Apache Solr(5.5)帮助文档(2)-关于
tags: Apache Solr(5.5)翻译
toc: true
---

>About This Guide

本帮助文档是用来描述Apache solr（开源搜索引擎解决方案）（5.5）。[下载地址]( http://archive.apache.org/dist/lucene/solr/ref-guide/)。
本文档的初衷是提供一份高水平的说明文档，因此目的是提供一份涉及多方面的帮助文档的而不仅仅是一本cookbook。从结构来看，本文档涉及的范围从初学者的了解需求到经验丰富的开发者扩展应用和解决问题的要求。因此，在应用开发的任何阶段，如果需要关于Solr的权威知识，可以参阅本文，可以从中获得很大帮助。
下面的内容假设你已经熟悉了一些搜索的概念，并且可以阅读XML文件。并不一定要了解JAVA开发，但是当直接操作 Lucene或是在Lucene/Solr基础上进行自定义扩展时，懂得Java还是有很大帮助的。

### 注意事项

![注意事项](http://upload-images.jianshu.io/upload_images/1213316-1a69c710487f704a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**Information**：蓝黑色背景，用于标记重点知识
**Notes**         ：黄色标记是当使用Solr时需要牢记于心的知识点
**Tip**              ：绿色背景，内容是一些有用的小技巧
**Warning**      ：红色背景，内容是一些警告信息。

### 主机和端口
运行Solr的默认端口是**8983 **。本文的例子、URL、截屏可能会有不同的端口号，是因为端口号可以进行修改。如果没有自定义你的Solr的配置，在运行示例时请确保可以使用**8983**端口号，或是修改配置，运行例子中的端口号。关于修改端口号的信息，请参看**管理Solr**章节
同样，默认的ULR都是'localhost'，如果你使用的是远程服务器的Solr，将'localhost'替换为相应的IP或是地址。

### 路径
路径信息与`solr.home`有关。`solr.home`位于Solr的主要安装目录，里面存储了Solr集合以及他们的配置文件和数据信息。当运行教程中例子时，`solr.home`会是在对应的例子example目录下的子目录，对应的目录会自动创建。
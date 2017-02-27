---
title : Apache Solr(5.5)帮助文档(26)-使用Solr管理交互界面—指定Core的工具—Schema浏览界面
tags : Apache Solr(5.5)翻译
toc : true
---

> Schema Browser Screen

Schema浏览界面通过浏览器窗口查看schema的数据。如果你是从Analysis界面跳转到Schema浏览界面，它会打开指定的field、dynamic field rule或是field type的界面。如果没有选中任何值，可以通过下拉菜单来选中一个field或是field type。

![Schema Browser Screen](http://upload-images.jianshu.io/upload_images/1213316-3e94b73a77168b3b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


Schema浏览界面对每一个field都提供了大量的有用信息。在上面的例子中，我们选中了`text` field。在中间窗口的右侧，展示了field名称，以及一系列构成`text`field的其他field。根据定义，这些field会被复制到`text`field。点击这些field中的一个，可以看到该field的定义信息。也可以看到field type，当然也可以查看对应的field type的定义信息。

在中间窗口的左侧，可以看到field type，以及field的定义信息。也可以查看到具有这个field的document数量。还有用于索引和查询的analyzer。点击这些内容左侧的图标，可以查看正在使用的tokenizer和filter的定义信息。当前看到的这些处理过程的输出结果就是在**Analysis Screen**界面测试某个指定的field的内容如何被处理时的看到的信息。
在analyzer信息下面，有一个**Load Term Info**按钮。点击这个按钮可以查看当前这个field中索引的前N个单词（terms）。点击任意一个单词（terms），将会前往**Query Screen**界面，查看该field值为这个单词（terms）的查询结果。如果想要每次都看到这些单词（term）统计结果，选中**Autoload**选择框，如果对应的field中有单词（term），这些信息就会展示出来。边上的直方图按照给定的频率大小展示对应的单词（terms）数量统计。
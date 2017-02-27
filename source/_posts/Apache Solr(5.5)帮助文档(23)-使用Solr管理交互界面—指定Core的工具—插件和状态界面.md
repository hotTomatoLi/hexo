---
title : Apache Solr(5.5)帮助文档(23)-使用Solr管理交互界面—指定Core的工具—插件和状态界面
tags : Apache Solr(5.5)翻译
toc : true
---

> Plugins & Stats Screen


插件界面展示了Solr运行状态和性能的信息以及数据统计。从中可以看到Solr缓存性能信息、Solr搜索的状态、searchHandler和requesthandler的配置。

在右侧出现的面板中，选择感兴趣的菜单，然后在屏幕中央面板上出现的名字中，点击任意一个，可以看到更多详细信息。在本例中，选择查看对应Core的搜索器的状态

![Searcher Statistics](http://upload-images.jianshu.io/upload_images/1213316-6e9c73e3ed97fb13.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

页面上展示的内容是在页面加载时得到的快照，可以通过选择点击**Watch Changes** 或是 **Refresh Values**来更新数据状态。**Watch Changes**会将变更的区域高亮展示，**Refresh Values**会重新加载页面获得最新的信息。
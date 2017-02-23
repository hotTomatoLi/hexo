---
title : Apache Solr(5.5)帮助文档(13)-使用Solr管理交互界面—Cloud页面
tags : Apache Solr(5.5)翻译
toc : true
---

>Cloud Screens

当运行在SolrCloud模式下，在交互界面中，**日志**和**Core管理**之间会增加一个选项。现阶段，通过交互界面管理SolrCloud集群是不可行的。但是可以通过交互界面查看集群，并打开每个节点来查看节点的状态和统计数据，以及每一个节点上的每一个Core。

注意：当使用SolrCloud时是只读的
"Cloud"菜单只在Solr实例运行在SolrCloud模式下才有效。单个节点或是主节点都不会展示这个选项。
点击左侧导航的Cloud选项，一个由"Tree"、"Graph"、"Graph (Radial)" 和 "Dump"组成的菜单会展示出来。默认视图"Graph"展示了每一个collection的图形，包括构成这些collection的shards，以及每一个shard的每个replica的地址。下面的图片展示了可以通过运行`bin/solr -e cloud`脚本启动示例的图形，包括两个节点，两个shard，两个cluster的集群。

![集群示例.png](http://upload-images.jianshu.io/upload_images/1213316-4d1a4cbe6632b81b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 "Graph (Radial)" 选项从另一个角度来展示每个节点。同样的例子，"radial graph"的结果如下

![Graph (Radial)集群](http://upload-images.jianshu.io/upload_images/1213316-3351e05e5da94982.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 "Tree" 选项展示了ZooKeeper下的目录结构，包括clusterstate.json，配置文件，和其他的状态信息文件。下图中，我们展示了"gettingstarted"collection中"shard1"中的leader定义。

![Tree](http://upload-images.jianshu.io/upload_images/1213316-9a0d8d3c476fba9d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

最后的选项为"Dump"，会让你下载一个包含所有ZooKeeper的配置信息的XML文件。
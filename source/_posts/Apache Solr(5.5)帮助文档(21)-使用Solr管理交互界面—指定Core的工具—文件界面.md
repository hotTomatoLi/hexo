---
title : Apache Solr(5.5)帮助文档(21)-使用Solr管理交互界面—指定Core的工具—文件界面
tags : Apache Solr(5.5)翻译
toc : true
---

文件界面允许你查看所选择的core的不同的配置文件（例如solrconfig.xml）。

![文件界面](http://upload-images.jianshu.io/upload_images/1213316-68cb2717788718b5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

solrconfig.xml文件定义了Solr在索引数据和响应请求的行为，schema.xml定义了数据的类别(field类别)，document需要分解的field，以及根据输入document的filed名称生成的动态field。任何在使用中的其他的配置文件会在solrconfig.xml或是schema.xml中引用。
配置文件在页面中无法编辑，必须使用文本编辑器。

这个页面与**Schema浏览页面**相关，这俩个页面都可以展示shema中的信息。**Schema浏览页面**提供了研究分析链的方式，并且展示了field的类别、field、动态field规则之间的关系。

在solrconfig.xml和schema.xml中定义的其他的配置选项在文档剩余的部分中都有描述。你可以特别注意一下几个章节：
- Indexing and Basic Data Operations
- Searching
- The Well-Configured Solr Instance
- Documents, Fields, and Schema Design
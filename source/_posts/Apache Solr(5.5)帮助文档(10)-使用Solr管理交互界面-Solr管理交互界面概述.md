---
title: Apache Solr(5.5)帮助文档(10)-使用Solr管理交互界面—Solr管理交互界面概述
tags: Apache Solr(5.5)翻译
toc : true
---

>Overview of the Solr Admin UI

Solr提供了一个Web接口，使Solr管理员和开发人员更容易的使用查看Solr配置信息，通过查询和分析document的field调整Solr配置，获取在线documentation和其他帮助。

![Solr管理交互界面](http://upload-images.jianshu.io/upload_images/1213316-52b73bc63c2bfba5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

访问`http://localhost:8983/solr/`可以查看主面板，面板包括两方面：
在屏幕左侧，Solr的logo下面是一个菜单栏。这个菜单栏提供了交互界面的所有页面的导航。上半部分链接内容指向系统信息和配置、日志、Core管理、Java配置和其他内容。下半部分是Solr实例的配置管理信息。选中一个Core后，会展示一个二级菜单。这个菜单用于展示对应Core的信息和配置选项等，菜单内容包括：Schema，配置，插件，查询数据等。

页面的中心部分展示了选中的详细信息。这部分可能的内容是参数的二级导航、文本内容、请求数据的图形化展示。详细信息请查看对应页面的说明文档。

实际上，Solr管理界面使用了和其他客户端访问获取Solr数据同样的 HTTP API 来展示图形界面。

注意：Solr管理交互界面的地址为`http://hostname:port/solr`，在当前版本（5.5）该地址会被重定向到`http://hostname:port/solr/#/ `。当然，也支持快捷重定向，只需要通过`http://hostname:port/`也会重定向到`http://hostname:port/solr/#/`，从而访问管理界面。

### 在solrconfig.xml配置管理界面
在solrconfig.xml文件中，<admin>标签定义了在特定Core查询页面的默认查询语句。默认值是`*:*`，将会查询所有的文档。本例中，改为`solr`。
```
<admin>
<defaultQuery>solr</defaultQuery>
</admin>
```

### 相关主题
- Configuring solrconfig.xml
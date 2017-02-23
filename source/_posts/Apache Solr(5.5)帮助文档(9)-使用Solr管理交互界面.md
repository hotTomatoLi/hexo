---
title: Apache Solr(5.5)帮助文档(9)-使用Solr管理交互界面
tags: Apache Solr(5.5)翻译
---

>Using the Solr Administration User Interface

这部分内容介绍了Solr管理交互界面（ the Solr Administration User Interface ，Admin UI）。
**Solr管理交互界面概述**介绍了交互界面的初始管理界面的基本特征，以及如果配置界面。此外，对管理界面的每个页面，都有单独的章节进行描述。
- **获得帮助** 介绍如何获得关于交互界面的更多信息
- **日志** 介绍了不同的日志级别，以及调用他们的方式
- **Cloud页面** 在运行SolrCloud模式时，展示关于节点的信息
- **Core管理**  介绍如何获取每个Core的管理信息
- **JAVA配置** 介绍每个Core的JAVA信息
- **线程 dump** 展示每一个线程的详细信息，和状态信息
-  **指定Core的工具** 是每个命名了的Core可使用的功能页面集的介绍
  -  **分析** 分析指定field的数据
  - **Dataimport** 展示**Data Import Handler**的状态
  - **Documents** 提供一个可以在浏览器里执行不同Solr索引命令的表单
  - **文件** 展示当前Core的配置信息，例如 solrconfig.xml 和 schema.xml。
  - **Ping** ping一个指定的Core，来判断该Core是否处于活动中
  - ** Plugins/Stats** 展示插件的统计数据，还有其他已经安装的组件
  - **查询**  运行提交关于一个Core的不同元素的结构化查询
  - **Replication** 展示当前Core的replication的状态，并运行启用/禁止replication
  - **Schema 浏览** 在浏览器窗口展示schema
  - ** Segments 信息** 为潜在的Lucene索引segment提供可视化界面
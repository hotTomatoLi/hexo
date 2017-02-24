---
title : Apache Solr(5.5)帮助文档(17)-使用Solr管理交互界面—指定Core的工具
tags : Apache Solr(5.5)翻译
toc : true
---

>Core-Specific Tools

在左侧的导航栏，可以看到一个名字为"Core Selector"的下拉菜单。点击该菜单会展示Solr的Core的列表，以及一个可用来搜索指定Core的搜索框（当有很多Core会很方便）。当选中了一个Core，一个二级菜单会在Core的名称下展示，通过该菜单可以对该Core进行管理。

一旦选中了一个Core，页面中间区域会展示该Core统计信息和其他信息。可以通过定义一个包含链接或是其他想要展示的信息的admin-extra.html文件，在主页面的Admin Extra"展示相应信息。

左边栏Core名称下面的链接，可以用于展示选择Core信息，和提供相应的选项。指定Core的选项在下面列出，可以点击相应的链接了解更多。

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
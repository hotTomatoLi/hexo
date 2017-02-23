---
title: Apache Solr(5.5)帮助文档(7)-开始-更进一步
tags: Apache Solr(5.5)翻译
toc: true
---

>A Step Closer

当前你已经知道一些关于Solr的schema的知识了，这部分内容将会描述Solr的home目录和其他的一些配置。
当Solr在服务器上运行时，它需要访问一个home目录。home目录里包含了重要的配置信息和存储的索引内容。home目录的结构在单机模式和SolrCloud模式下会有一些不同。
Solr的home目录的关键部分如下面的例子。

**单机模式**

![单机模式](http://upload-images.jianshu.io/upload_images/1213316-cac4b6d3ca4f0c01.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**SolrCloud模式**


![SolrCloud模式](http://upload-images.jianshu.io/upload_images/1213316-c333c247b76d7365.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


也许会看到其他的一些文件，但是需要掌握的主要的文件如下：
- solr.xml 表明了你Solr服务器运行实例的配置信息。关于Solr.xml的更多信息，可以查看Solr Cores and solr.xml章节
- 每个Solr的Core
  - core.properties 定义了每个core的特定属性，例如名字，core所属的集合，schema文件的位置和其他参数。更多关于core.properties的细节，清查看Defining core.properties。
  - solrconfig.xml 控制更高级别的行为。例如，可以指定数据目录的位置。更多关于solrconfig.xml的信息，可以查看Configuring solrconfig.xml。
  - schema.xml （或是managed-schema）描述了Solr需要索引的documents。在schema.xml里，可以定义document，实际上时一些字段的集合。需要定义字段的类别和字段本身。字段类别定义是很重要的，它包括了Solr如何处理输入字段值和查询值。更多关于schema.xml的信息，可以看Documents，Fields，and Schema Design。如果使用的是Solr的Schema API来管理字段，你需要查看managed-schema而不是schema.xml（看Managed Schma Definition in SolrConfig获取更多信息）。
  - data/ 包括低级别索引文件的目录。

需要注意的是，SolrCloud示例在每个Solr Core里不包括conf目录（就没有solrconfig.xml或是schema.xml文件）。这是因为在conf目录下的配置文件保存在Zookeeper中，从而可以在集群中传播。
如果使用的是SolrCloud以及嵌入式Zookeeper实例，你会看到Zookeeper配置zoo.cfg和数据文件zoo.data。然而，如果使用的是自己的Zookeeper，将会将你自己的Zookeeper中的配置应用到Solr中，而Solr中的复制文件将不会被使用。更多关于Zookeeper和SolrCloud信息，请查看SolrCloud章节。
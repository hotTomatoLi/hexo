---
title: Apache Solr(5.5)帮助文档(1)-目录
tags: Apache Solr(5.5)翻译
toc: true
---

> Apache Solr Reference Guide

本帮助文档是用来描述Apache solr，一个开源搜索引擎解决方案（5.5版本）。原文[下载地址]( http://archive.apache.org/dist/lucene/solr/ref-guide/)。
内容列表如下：
### 开始
这部分内容指导你安装并且设置Solr。
### 使用Solr管理交互界面
这部分介绍了Solr的web界面。通过浏览器，可以查看配置文件、提交查询、查看日志文件设置、查看Java环境设置、监控并且管理已经发布的配置。
### Documents, Fields, and Schema Design
这部分内容描述了Solr如何构建它的数据，使其可以被索引。其中，介绍了如何定义**fields** 和 **field types**。**fields** 和 **field types**被Solr用于在**document**中组织数据，而**document**被Solr用于索引数据。
### 理解 Analyzers, Tokenizers, and Filters
这部分介绍了Solr如何准备被索引和搜索的文本。**Analyzers**解析文本，输出**tokens**和**lexical units**（**tokens**和**lexical units**被用于索引和搜索）。
**Tokenizers**将**field**数据分解成**tokens**。**Filters**则根据**token**流，进行一些其他的选择或是转换的工作。

### 索引和基本的数据操作
这部分描述了索引的流程和基本的索引操作，例如提交、优化和回滚。

### 搜索
这部分从总体上描述了Sol的查询过程。它描述了查询中的主要组件，包括请求处理器（**request handlers**）、查询解析器（**query parsers**）、响应输出（**response writers**）。在这部分内容中，列出了Solr搜索可以使用的参数，也描述了一些可以用来调整搜索结果的特点，如**boosting**和**faceting**。

### 配置好的Solr实例
这部分介绍了Solr的性能调优。首先，从整体上介绍了solrconfig.xml文件，然后介绍如何通过修改solr.xml文件来修改**core**的配置，如何修改**Luncene index writer**的配置，等等。

### 管理Solr
在这部分内容中，主要讨论了运行和监控Solr的主题。也包括其他的主题，如何备份Solr实例，以及如何用Java Management Extensions (JMX) 运行Solr.

### SolrCloud
这部分描述了Solr新特征中最新和最令人兴奋的特征—SolrCloud。SolrCloud使Solr具有综合的分布式能力。

 ### 遗留系统和分布式
这部分讲述了建立Solr分布式的过程。首先将一个大的索引，分成小的部分（称为**shards**），然后将**shards**在多个服务器端进行分发，或是通过多个服务复制同一个索引数据。

### 客户端API
这部分描述了如何通过客户端API操作Solr（Javascript，JSON，Ruby）。
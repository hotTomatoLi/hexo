---
title: Apache Solr(5.5)帮助文档(3)-开始-说明
tags: Apache Solr(5.5)翻译
toc: true
---

>Getting Started

Solr极大的方便了开发者开发复杂高效的搜索应用，并提供相应的高级特性例如**faceting**（按列排版搜索结果，并表明关键元素的数字）。Solr依赖另一个开源的搜索技术：Lucene。Lucene是一个Java库，提供了索引、搜索、拼写检查、高亮命中结果、高级分词能力等。Solr和Lucene都由 Apache Software Foundation 管理。
Lucene项目当前排在所有开源项目的前15，在Apache项目中排前5，大概有超过4000家公司使用。在过去三年，Lucene/Solr的下载量增加了近10倍，现在的频率大约是每天6000次。Solr搜索引擎，提供了基于Lucene库的搜索平台应用，是Lucene中增长最快的子项目。Lucene和Solr为开发搜索和发现的软件供应商提供了一个具有竞争力的选择。
这部分内容会帮助你迅速的设置好并运行Solr，并介绍Solr基本的结构和特征。它包括了一下内容：
- 安装Solr：Solr安装的流程介绍
- 运行Solr：运行Solr的介绍，包括启动服务器，增加document，运行索引。
- 快速概览：Solr工作原理的概览
- 更进一步：Solr主目录和配置项的介绍
- Solr启动脚本介绍：bin/solr下脚本命令和参数的完整介绍
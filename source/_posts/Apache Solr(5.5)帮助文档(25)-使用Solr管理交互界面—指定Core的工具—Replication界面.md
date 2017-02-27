---
title : Apache Solr(5.5)帮助文档(25)-使用Solr管理交互界面—指定Core的工具—Replication界面
tags : Apache Solr(5.5)翻译
toc : true
---

> Replication Screen

Replication界面展示了指定名字的Core的副本状态。SolrCloud已经取代了本界面的大部分功能，但如果你仍在用主-从索引机制，你可以在本界面进行如下操作：
1. View the replicatable index state. (on a master node)
2. View the current replication status (on a slave node)
3. Disable replication. (on a master node)

注意：当使用SolrCloud时需要注意
When using SolrCloud, do not attempt to disable replication via this screen.

更多关于如何配置replication的内容在**Index Replication**章节
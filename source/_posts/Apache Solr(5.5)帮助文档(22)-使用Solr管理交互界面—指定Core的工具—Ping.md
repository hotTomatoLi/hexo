---
title : Apache Solr(5.5)帮助文档(22)-使用Solr管理交互界面—指定Core的工具—Ping
tags : Apache Solr(5.5)翻译
toc : true
---

> Ping

在一个Core名称下选择Ping，是向服务器发起一个`ping`请求，来检测服务器是否启动。
通过在`solrconfig.xml`文件中使用`requestHandler`，可以配置Ping的内容。
```
<!-- ping/healthcheck -->
<requestHandler name="/admin/ping" class="solr.PingRequestHandler">
    <lst name="invariants">
        <str name="q">solrpingquery</str>
    </lst>
    <lst name="defaults">
        <str name="echoParams">all</str>
    </lst>
<!-- An optional feature of the PingRequestHandler is to configure the
handler with a "healthcheckFile" which can be used to enable/disable
the PingRequestHandler.
relative paths are resolved against the data dir
-->
<!-- <str name="healthcheckFile">server-enabled.txt</str> -->
</requestHandler>
```

Ping选项并不会打开一个新页面，但是如果选择了某一个core，可以在core总览页面查看相应的ping的request的状态。请求花费的时间会在Ping选项的后面展示，单位是毫秒。
例子如下：
**输入**
```
http://localhost:8983/solr/<core-name>/admin/ping
```
该命令会向相应名称的core发送ping请求，来得到响应。

**输入**
```
http://localhost:8983/solr/<collection-name>admin/ping?wt=json&distrib=true&indent=true
```
该命令会向给定名称的collection的所有副本发送请求从而获得响应。

**实例输出**
```
<response>
    <lst name="responseHeader">
        <int name="status">0</int>
        <int name="QTime">13</int>
        <lst name="params">
            <str name="q">{!lucene}*:*</str>
            <str name="distrib">false</str>
            <str name="df">_text_</str>
            <str name="rows">10</str>
            <str name="echoParams">all</str>
        </lst>
    </lst>
    <str name="status">OK</str>
</response>
```
前面的请求都会有上述输出结果。输出的status值是OK表明对应的节点成功响应请求。

**使用SolrJ的示例**
```
SolrPing ping = new SolrPing();
ping.getParams().add("distrib", "true"); //To make it a distributed request against
a collection
rsp = ping.process(solrClient, collectionName);
int status = rsp.getStatus();
```
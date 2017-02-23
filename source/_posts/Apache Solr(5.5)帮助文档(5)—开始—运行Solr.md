---
title: Apache Solr(5.5)帮助文档(5)-开始-运行Solr
tags: Apache Solr(5.5)翻译
toc: true
---

>Running Solr

本章节介绍了根据示例schema运行Solr，增加document，查询信息。

### 启动服务器
安装完毕后，可以通过在Solr目录下运行bin/solr来运行Solr。
```
$ bin/solr start
```
Windows用户，可以通过运行bin/solr.cmd命令运行。
```
bin\solr.cmd start
```
上述命令命令会在后台启动Solr，监听端口号8983。
当在后台运行Solr时，启动脚本会确保Solr正确启动，才会返回到命令行。
 bin/solr 和 bin\solr.cmd脚本运行自定义启动内容。下面来看一些 bin/solr脚本的例子（如果使用的 是Windows平台，使用的是bin\solr.cmd）

#### Solr脚本参数
bin/solr 有如下脚本参数
##### 脚本Help
查看使用帮助，执行
```
$ bin/solr -help
```
如果需要查看start命令的专门的使用说明，执行
```
 $ bin/solr start -help
```
##### 在前台启动Solr
Solr是个服务，因此通常会在后台运行，尤其是在Linux/Unix上。但是，如果要在前台启动，执行
```
 $ bin/solr start -f
```
如果运行的是Windows平台，执行
```
bin\solr.cmd start -f
```
##### 在不同的端口启动
-p参数可以修改Solr监听的端口，例如
```
 $ bin/solr start -p 8984
```

##### 停止Solr
当在前台运行Solr时（用-f参数），可以通过`ctrl+c`来停止。但是，如果是在后台运行，停止命令如下：
```
bint/solr stop -p 8983
```
停止命令需要表明监听的Solr端口。也可以通过`-all`参数，来停止所有的Solr实例。

##### 指定启动时的示例配置
Solr也提供一系列有用的例子来帮助你学习它的主要特点。通过-e 标志可以加载相应的示例。例如，如果要加载"techproducts"例子，命令如下：
```
bin/solr -e techproducts
```
目前已经存在的示例包括：techproducts，dih，schemaless，cloud。如果想要了解关于各个示例的更多细节，可以查看Running with example configurations部分。
**informations**：开始运行SolrCloud
运行cloud示例会在SlorCloud模式下运行Solr。关于更多关于云模式运行的Solr信息，可以查看**开始SolrCloud**。

##### 检测Solr是否在运行
如果不确定Solr是否在运行，可以使用状态命令：
```
bin/solr status
```
这个命令将会搜索运行在当前电脑上的所有Solr示例，并将他们的基本信息聚集在一起（例如版本和内存使用等等）。
上述内容就可以查看Solr是否在运行，如果需要确认，可以通过Web浏览器查看管理页面。
!!!!!Solrweb图
如果Solr没有运行，浏览器会报错，错误信息为不能连接到服务器。请检测端口号，然后再重试。

### 创建core
如果没有指定示例配置，需要创建一个core，才能索引和搜索。脚本命令如下：
```
bin/solr create -c <name>
```
这样会创建一个core。这个core会使用data-driven schema，将会在你新增documents时判断field type。
如果需要查看创建新core时所有的参数，可以执行
```
bin/solr create -help
```

### 新增Documents
Solr的目的是查找符合条件的documents。Solr的schema阐释了它的内容的存储结构，但是没有documents的话，是没有东西来进行搜索的。Solr首先需要有输入内容。
在索引你自己需要的内容之前，你可能需要增加一些示例documents。Solr安装程序自带了一些不同类别的示例documents。位置在安装目录下的example子目录下。
在bin目录下，是提交脚步。命令行工具可以用来索引不同的documents。现在先不要考虑态度的细节，在Indexing and Basic Data Operations章节，会有关于索引的所有细节。
`-help`命令可以查看关于`bin/post`的更多信息。Windows用户，可以查看Post Tool on Windows章节。
`bin/post`可以提交多种形式的内容到Solr上，包括Solr原始的XML文件和JSON格式文件，CSV格式文件，富文档的字典树，甚至一个简短的web crawl。通过在`bin/post -help`最后不同类别的命令示例，可以轻松的开始提交你自己的内容到Solr上。
下面继续将所有的documents按照示例的XML文件格式添加。
```
./bin/solr create -c gettingstarted
```
```
./bin/post -c gettingstarted example/exampledocs/*.xml
/usr/local/java/jdk1.8.0_111/bin/java -classpath /home/lienquan/usr/solr-5.5.0/dist/solr-core-5.5.0.jar -Dauto=yes -Dc=gettingstarted -Ddata=files org.apache.solr.util.SimplePostTool example/exampledocs/gb18030-example.xml example/exampledocs/hd.xml example/exampledocs/ipod_other.xml example/exampledocs/ipod_video.xml example/exampledocs/manufacturers.xml example/exampledocs/mem.xml example/exampledocs/money.xml example/exampledocs/monitor2.xml example/exampledocs/monitor.xml example/exampledocs/mp500.xml example/exampledocs/sd500.xml example/exampledocs/solr.xml example/exampledocs/utf8-example.xml example/exampledocs/vidcard.xml
SimplePostTool version 5.0.0
Posting files to [base] url http://localhost:8983/solr/gettingstarted/update...
Entering auto mode. File endings considered are xml,json,jsonl,csv,pdf,doc,docx,ppt,pptx,xls,xlsx,odt,odp,ods,ott,otp,ots,rtf,htm,html,txt,log
POSTing file gb18030-example.xml (application/xml) to [base]
POSTing file hd.xml (application/xml) to [base]
POSTing file ipod_other.xml (application/xml) to [base]
POSTing file ipod_video.xml (application/xml) to [base]
POSTing file manufacturers.xml (application/xml) to [base]
POSTing file mem.xml (application/xml) to [base]
POSTing file money.xml (application/xml) to [base]
POSTing file monitor2.xml (application/xml) to [base]
POSTing file monitor.xml (application/xml) to [base]
POSTing file mp500.xml (application/xml) to [base]
POSTing file sd500.xml (application/xml) to [base]
POSTing file solr.xml (application/xml) to [base]
POSTing file utf8-example.xml (application/xml) to [base]
POSTing file vidcard.xml (application/xml) to [base]
14 files indexed.
COMMITting Solr index changes to http://localhost:8983/solr/gettingstarted/update...
Time spent: 0:00:01.243
```
这样，Sol就索引了示例文档。

### 提问
现在你已经获得了索引好的文档，可以进行语句查询了。最简单的方式是拼接带参数的URL，与拼接其他的HTTP URL的方式是一样的。
例如，下面的查询在所有的字段搜索"video"。
```
http://localhost:8983/solr/gettingstarted/select?q=video
```
我们注意到URL里包括了主机名称（localhost），服务器监听的端口号（8983），应用名称（solr），查询的请求handler（select），和查询语句（q=video）。
上述查询结果包含在一个XML文档中，可以通过直接点击上述URL进行查看。这个XML文档包括两部分，第一部分是响应头（responseHeader），其中包括了响应本身的信息。第二部分是主要的部分，存储在`result`标签内。`result`标签里可能包含一个或多个`doc`标签，每个`doc`标签里存储了符合查询条件的document的字段。可以通过标准的XML转换技术，将Solr的查询结果转变成适合展示的格式。此外，Solr可以将查询结果按照JSON，PHP，Ruby甚至用户自定义的格式输出。
如果万一你在阅读本文档时没有运行Solr，下面的截图展示了一个查询的结果（实际上是下个例子）。浏览器为火狐浏览器。最顶层的节点`response`包含了一个`lst`节点（名称属性为“responseHeader”）和一个`result`节点（名词属性为"response"）。在`result`节点里，你可以看到表述查询结果的documents。


![查询结果](http://upload-images.jianshu.io/upload_images/1213316-3f0e753f79b44c38.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

一旦你掌握了查询的基本概念，你可以很容易的进行查询功能扩展。下面例子的查询与前面的一致，只是结果只包括ID，name，price字段的数据。如果在查询条件中不明确指出需要的字段，所有的字段都会返回。
```
http://localhost:8983/solr/gettingstarted/select?q=video&fl=id,name,price
```
下面的例子搜索只在name字段值为"black"的数据。如果在查询时不指定搜索的字段，Solr会搜索默认字段，该字段会在schema里配置。
```
http://localhost:8983/solr/gettingstarted/select?q=name:black
```
也可以指定字段值的范围。下面的查询条件搜索price字段在0到400之间的数据。
```
http://localhost:8983/solr/gettingstarted/select?q=price:[0%20TO%20400]&fl=id,name,price
```

**Faceted**浏览是Solr的一个核心特征，它允许用户按照对各自应用有用途的方式精简查询结果。例如，一个购物网站可以提供**facets**从而在制造商或是价格方面精简查询结果。
**Faceting**信息返回时作为Solr查询结果的第三部分。可以通过下面的查询条件来看一下效果。在这个查询中，增加了`facet=true`和`facet.field=cat`。
```
http://localhost:8983/solr/gettingstarted/select?q=price:[0%20TO%20400]&fl=id,name,price&facet=true&facet.field=cat
```
在查询结果中，除了熟悉的`responseHeader`节点和`response`节点，还有一个`facet_counts`节点。下面的XML文档将`responseHeader`节点和`response`节点收缩了，我们可以更清晰的看到**faceting**信息。
```
<response>
<lst name="responseHeader">
<int name="status">0</int>
<int name="QTime">205</int>
<lst name="params">...</lst>
</lst>
<result name="response" numFound="13" start="0">
<doc>...</doc>
<doc>...</doc>
<doc>...</doc>
<doc>...</doc>
<doc>...</doc>
<doc>...</doc>
<doc>...</doc>
<doc>...</doc>
<doc>...</doc>
<doc>...</doc>
</result>
<lst name="facet_counts">
<lst name="facet_queries"/>
<lst name="facet_fields">
<lst name="cat">
<int name="electronics">9</int>
<int name="connector">2</int>
<int name="hard drive">2</int>
<int name="memory">2</int>
<int name="search">2</int>
<int name="software">2</int>
<int name="camera">1</int>
<int name="copier">1</int>
<int name="electronics and stuff2">1</int>
<int name="multifunction printer">1</int>
<int name="music">1</int>
<int name="printer">1</int>
<int name="scanner">1</int>
<int name="currency">0</int>
<int name="electronics and computer1">0</int>
<int name="graphics card">0</int>
</lst>
</lst>
<lst name="facet_dates"/>
<lst name="facet_ranges"/>
<lst name="facet_intervals"/>
<lst name="facet_heatmaps"/>
</lst>
</response>
```
在**faceting**结果中，统计了每种cat字段值符合查询条件的数字。可以轻松的根据上述信息为用户提供更加便捷的方式来精简查询结果。可以在Solr请求中增加更多的过滤查询。下面的查询增加了software的策略，来限制查询结果。
```
http://localhost:8983/solr/gettingstarted/select?q=price:[0%20TO%20400]&fl=id,name,price,cat&facet=true&facet.field=cat&fq=cat:software
```

### 个人理解
`facet=true`开启了**facet**查询，`facet.field`指定了**facet**的字段，上述例子中，`facet.field=cat`，说明，首先统计所有的cat字段的值，得到cat值字段的分类，在符合查询条件的数据中，统计每种cat值分类的个数。`fa=cat:software`貌似是不显示cat值为software的统计。
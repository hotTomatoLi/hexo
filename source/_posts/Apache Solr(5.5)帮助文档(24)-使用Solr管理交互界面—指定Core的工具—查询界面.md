---
title : Apache Solr(5.5)帮助文档(24)-使用Solr管理交互界面—指定Core的工具—查询界面
tags : Apache Solr(5.5)翻译
toc : true
---

> Query Screen

你可以通过在每个Core下的**Query**，向Solr服务器提交查询请求，并分析结果。在示例中的截屏中，已经提交了查询，屏幕内容展示了发送回浏览器的查询结果（JSON格式）。

![Query Screen](http://upload-images.jianshu.io/upload_images/1213316-b0c001ef819e16d5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

查询的Core的名称是"techproducts"，使用的查询语句是默认的查询语句（在solrconfig.xml中定义的）"*:*"。这个查询命令会查询在这个Core中索引的所有记录。我们暂时先保持其他的参数为默认值，下面的表格解释了这些参数的含义，在本文档中的后续部分中，也介绍了这些参数的细节信息。

查询请求的响应在表单的右边展示。对Solr发起的请求时普通的HTTP请求，查询请求的内容在查询结果上方，以灰色字体展示。如果点击该内容，浏览器会打开一个新窗口，仅仅展示查询请求和响应内容（不包括Solr管理交互界面的其他内容）。响应的内容是以JSON格式展示，格式设置是请求的一部分（可以查看在请求结尾的`wt=json`参数）。

查询响应至少包含两部分数据，根据选择的参数不同，可能会包含更多的部分。肯定包含的两部分是响应头和响应内容。响应头里包括了查询状态（status）、查询时间（QTime）、发起查询时传递的参数（params）。

本界面运行你通过尝试不同的查询参数，观察索引的document。在表单下面可使用的查询参数是大多数用户可用的基本参数，此外，也有很多参数可以通过手动加入到查询请求中（在浏览器打开）。下面的表格列出了可用的参数：

|字段|描述|
|:--:|:--:|
|Request-handler(qt)|指定查询请求使用的查询handler。如果没有明确指定查询handler，那么Solr会使用标准查询handler来执行查询。|
|q|查询事件。请在**Searching**章节查看关于此参数的解释。|
|fq|过滤查询，请在**Common Query Parameters**章节查看关于此参数的更多信息。|
|sort|根据响应的结果的score或是其他的指定的字段，将查询响应排序（升序或降序）|
|start, rows|start是只查询偏移量，查询返回的数据从该位置的document开始，默认值为0，意思是查询结果将会从匹配查询条件的第一个document开始返回。该字段也可以输入符合开始查询参数语法的内容（在**Searching**章节介绍）。row是指需要返回的行数|
|fl|定义每个document返回的field。可以明确的列出你想要Solr返回的内容，包括存储的field、function、doc transformers，通过空格或是逗号进行分隔|
|wt|指定用于格式化查询结果的响应Writer。未明确定义，采用的是XML格式|
|indent|勾选这个选项后，响应Writer 会将响应内容进行压缩，使其可读性更高|
|debugQuery|勾选这个选项会要求查询结果带有调试信息，包括每个document返回的"explain info"。这些调试信息主要是为管理人员或是程序员准备的|
|dismax|勾选这个选项，会启用Dismax查询解析器。更多信息请查阅**The DisMax Query Parser**章节|
|edismax|勾选这个选项会启动Extended查询解析器。更多信息请查阅**The Extended DisMax QueryParser**章节|
|hl|勾选这个选项后，会启用查询响应中高亮功能。更多信息请查阅**Highlighting**章节|
|facet|启动faceting功能，会将查询结果根据索引的字段进行归类。更多信息请查阅**Faceting**章节|
|spatial|勾选该按钮，会允许使用本地数据，主要应用于物理空间搜索。Click to enable using location data for use in spatial or geospatial searches.更多信息请查阅** Spatial Search **章节。|
|spellcheck|勾选该按钮会启动Spellchecker，会基于其他类似的单词提供内联查询建议。更多信息请查阅**Spell Checking **章节。|

### 相关主题
**Searching**
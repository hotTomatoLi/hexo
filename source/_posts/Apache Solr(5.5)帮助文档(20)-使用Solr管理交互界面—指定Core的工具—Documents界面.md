---
title : Apache Solr(5.5)帮助文档(20)-使用Solr管理交互界面—指定Core的工具—Documents界面
tags : Apache Solr(5.5)翻译
toc : true
---

Documents界面提供了一个简单的表单组件，该组件允许你通过浏览器，使用不同的格式执行索引相关的命令。

![Documents界面](http://upload-images.jianshu.io/upload_images/1213316-3fae3b5549040297.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在该界面可以执行以下操作：
- 以JSON、CSV或XML格式复制documents，并且提交到索引；
- 上传documents（JSON、CSV和XML格式）文件；
- 通过选取fields和fields值构建documents；

首先定义使用的RequestHandler（也就是'qt'）。默认使用的是`/update`。如果要换成其他的Handler，例如使用Solr Cell，改变request handler为`/update/extract`。

然后选择Document类别，来定义载入的document的类别。剩下的参数会根据选取的document type不同而有所不同。

###JSON
使用JSON的document类别的功效与在命令行使用requestHandler时一样的。这些数据可以放在一个Document输入框中，而无须放到一个curl命令中。这些document数据的需要满足JSON格式。
输入完后，可以决定何时将document加入到索引；是否输入同样ID的document时，已经存在的数据会被覆盖（如果否，输入的数据会被丢弃）；以及if a document boost should be applied。

这些选项只能够新增索引，或是覆盖现有document；如果需要更新操作，请查看Solr Command 选项。

### CSV
使用CSV的document类别的功效与在命令行使用requestHandler时一样的。这些数据可以放在一个Document输入框中，而无须放到一个curl命令中。这些document数据的需要满足CSV格式。
输入完后，可以决定何时将document加入到索引；是否输入同样ID的document时，已经存在的数据会被覆盖（如果否，输入的数据会被丢弃）。

### Document Builder
Document Builder选项提供了一个向导界面来输入一个document的field。

### 文件上传
文件上传选项允许选择一个准备好的文件并上传。如果在Request Handler选项中选择`/update`，文件格式将被限制在XML、CSV和JSON中。
此外，如果要使用ExtractingRequestHandler（也就是Solr Cell），可以将选项值改为`/update/extract`。必须在solrconfig.xml中定义这个handler，定义好期望的默认值。同时需要更新的是ExtractingRequestHandler参数中的&literal.id，这样选择的文件才能指定唯一ID。

输入完后，可以决定何时将document加入到索引；是否输入同样ID的document时，已经存在的数据会被覆盖（如果否，输入的数据会被丢弃）。

### Solr Command
Solr Command选项允许你通过XML或JSON在documents上执行特殊的操作，例如决定document是否新增或是删除，更新documents的特定fields，或是在索引上执行和优化命令。
如果在命令行选择`/uppdate`选项，document需要按照规则组织。

### XML
使用XML的document类别的功效与在命令行使用requestHandler时一样的。这些数据可以放在一个Document输入框中，而无须放到一个curl命令中。这些document数据的需要满足XML格式，每个document之间用`<doc>`标签分隔，并且每个field都已经定义。
输入完后，可以决定何时将document加入到索引；是否输入同样ID的document时，已经存在的数据会被覆盖（如果否，输入的数据会被丢弃）。
这些选项只能够新增索引，或是覆盖现有document；如果需要更新操作，请查看Solr Command 选项。

### 相关的主题
- Uploading Data with Index Handlers
- Uploading Data with Solr Cell using Apache Tika
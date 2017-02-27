---
title : Apache Solr(5.5)帮助文档(18)-使用Solr管理交互界面—指定Core的工具—Analysis界面
tags : Apache Solr(5.5)翻译
toc : true
---

>Core-Specific Tools

Analysis界面会根据schema.xml中的field，field类别，动态规则等配置，向你展示数据是如何处理。你可以分别在索引或是在查询过程中，查看数据是如何处理的，也可以在同一时间查看数据处理。理想情况下，你会希望数据内容被连贯处理。这个界面允许你在field类别或是filed的分析链中验证配置。

在页面顶部的俩个输入框中，输入一个或俩个都输入，然后选择用于分析的filed或是filed 类别定义。

![Anlysis界面](http://upload-images.jianshu.io/upload_images/1213316-2abfbf136c5eaa36.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


如果勾选了**Verbose Output**选择框，讲会看到更多信息，包括输入值的转换（例如，转成小写，去除额外的字符），字节信息，类别，详细位置信息。根据配置文件中的field和field的类别的不同，展示的信息也会有很大的不同。分析过程的每一步都会在不同的区域展示，开头是应用在该步上的tokenier或是filter的缩写。鼠标移动或是点击缩写名称，就可以看到对应的tokenier或filter的名称和路径。


![v](http://upload-images.jianshu.io/upload_images/1213316-e01f40dfd3eaaec7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
在上面的截图示例中可以看到，在输入值"Running is a sport"上，应用了多个信息转换器。单词"is"和"a"都被删除，"running"变为基本形式"run"。这是因为，在上述场景中，我们使用的field类别是`text_en`，该类别是用于移除停止符单词（通常是没有什么实际含义的小单词）和"stem" terms（词干？？），可以用于找到更多可能的匹配（尤其对复数单词的场景很有效）。如果点击在**Analyze Fieldname/Field Type**下拉菜单边上的问号标识，会打开Schema浏览窗口，展示这个field的配置。

**理解 Analyzers, Tokenizers, and Filters**部分详细描述了每一个选项的含义，以及它将如何转换你的数据。**运行你的 Analyzer**部分包含了使用Analysis界面的示例。
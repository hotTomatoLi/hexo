---
title: hexo-增加目录
tags: hexo
toc: true
---

本文主要是针对yilia主题增加目录显示，其他主题未测，参考地址[这里](http://www.jianshu.com/p/ac853e1afedb)

### 修改文章显模板
文件地址themes\yilia\layout\_partial\article.ejs。
在`<%- post.content %>`前增加
```
		<% if (!index && post.toc){ %>
		<div id="toc" class="toc-article">
		<strong class="toc-title">文章目录</strong>
			<%- toc(post.content) %>
		</div>
		<% } %>
```
### 修改md文件
在每个md文件头，增加`toc: true`值即可。

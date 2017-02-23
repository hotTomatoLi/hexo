---
title: Apache Solr(5.5)帮助文档(4)-开始-安装Solr
tags: Apache Solr(5.5)翻译
toc: true
---
>Installing Solr

这部分介绍了如何安装Solr。Solr可以运行在任意一个可使用Java运行环境（JRE）的系统上，当前包括Linux、OS/X和Windows。下面的介绍在上述平台都成立，除了在标记中说明的一些在Windows平台的特例。

### 安装Java
Solr要求JRE 1.7及以上版本。命令行里检查Java版本命令如下：
```
$ java -version
java version "1.8.0_60"
Java(TM) SE Runtime Environment (build 1.8.0_60-b27)
Java HotSpot(TM) 64-Bit Server VM (build 25.60-b23, mixed mode)
```
上述结果可能有所不同，但是需要确保最低版本要求。我们推荐使用未停止更新的版本，如果没有要求的版本，或是找不到Java命令行，可以从Oracle官网下载最新的版本。
 ### 安装Solr
可以从[Solr网站](http://lucene.apache.org/solr/)下载Solr。
Linux/Unix/OSX系统下载.tgz文件，Windows系统下载.zip文件。一旦开始，只需要解压下载的文件中。如果需要设置生产环境的Solr，请参照在Taking Solr to Production 页面的说明。现在仅仅需要解压Solr文件到你指定的主目录，在Linux下可以
```
$ cd ~/
$ tar zxf solr-5.0.0.tgz
```
一旦解压完毕，可以按照运行Solr章节的内容运行Solr。
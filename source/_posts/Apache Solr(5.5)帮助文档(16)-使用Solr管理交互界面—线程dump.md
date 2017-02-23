---
title : Apache Solr(5.5)帮助文档(16)-使用Solr管理交互界面—线程dump
tags : Apache Solr(5.5)翻译
toc : true
---

>Thread Dump

线程 dump页面可以让你观察到在你的服务器上活动的线程信息。页面上列出了每一个线程，并且可以查看它的堆栈信息。线程左边的图标表明了当前线程的状态：例如，绿色圆圈里对勾表明当前线程是"RUNNABLE"状态。在线程右边的下拉箭头表明，你可以点击展开来查看线程的堆栈信息。

![线程 dump](http://upload-images.jianshu.io/upload_images/1213316-1703d7fcd9a9d5fd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

当鼠标在线程名字上滑动时，会提示一个消息框，展示当前线程的状态。

|状态|含义|
|:--:|:--:|
|NEW|一个还未启动的线程|
|RUNNABLE|正在虚拟机中执行的线程|
|BLOCKED|线程阻塞，等待监控锁|
|WAITING|线程无限期等待另一个线程执行特定操作|
|TIMED_WAITING|线程在指定时间内等待另一个线程执行特定操作|
|TERMINATED|线程退出|


当点击一个可以展开的线程，可以看到堆栈信息，如下图所示：

![查看一个线程](http://upload-images.jianshu.io/upload_images/1213316-00ab80d48f736d1a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

也可以勾选**Show all Stacktraces**按钮，自动展示所有可以展开的线程。

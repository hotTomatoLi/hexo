---
title: web开发-火狐浏览器端请求取消-ajax的error回调
tags: [web开发]
toc: true
---

# 背景
在使用jquery(1.12)进行ajax请求数据时，在火狐浏览器下发生了下面的现象。   
ajax请求数据时，后台数据还未返回时，此时点击其他页面链接，在火狐下，ajax的error回调函数会执行，在回调中有一个弹出框提示请求数据出错；而在chrome下则正常跳转；
其实，如果不停的刷新该页面，也会产生这种问题。   
究其原因，就是jquery的ajax函数，将浏览器端的取消，也当做是错误；但是这种明显是用户取消的行为不应该作为错误处理。
此时，服务器端的会有提示：
```
org.apache.catalina.connector.ClientAbortException: java.io.IOException
```

# 参考资料
[和浏览器异步请求取消相关的那些事](http://blog.csdn.net/w1057742284/article/details/52166917)
[How to Distinguish a User-Aborted AJAX Call from an Error](http://ilikestuffblog.com/2009/11/30/how-to-distinguish-a-user-aborted-ajax-call-from-an-error/)
[handle ajax error when a user clicks refresh](http://stackoverflow.com/questions/699941/handle-ajax-error-when-a-user-clicks-refresh/18170879#18170879)

# 实际解决
## 问题描述
参考了上述文献，发现无论是服务器错误或是页面刷新，ajax的`error`函数里的`textStatus`都是`error`，也没有尝试其他的参数值。
```
error: function ( jqXHR, textStatus, errorThrown) {
               
}
```

## 同步请求
如果ajax请求为同步，那就不会允许在请求数据时在浏览器点击事件，也就不会有这种问题。即添加
```
async:false
```


## 监听`beforeunload`
监听window对象的`beforeunload`事件，增加一个标志flag。当页面跳转、刷新时，`flag=true`,在`error`事件中，如果`flag==true`，则不执行相关提示。
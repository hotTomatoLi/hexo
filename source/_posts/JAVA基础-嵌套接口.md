---
title: JAVA基础-嵌套接口
tags: JAVA基础
toc: true
---


# 背景
代码中经常有一些类或接口中的嵌套接口。

# 示例
```
public interface Map<K,V> {
    
    interface Entry<K,V> {
    
    }

}
```

# 说明
嵌套接口(nested interface)，本身就是`static`的，无论是否有`static`修饰。
嵌套接口的用途：
- 如果一个接口，只需要在outer的类中使用，那么嵌套接口就可以不用再新建一个文件（可能在事件监听方面用的较多，java awt或是Android）
```
/**
 * 嵌套接口
 */
public class NestedInterface {

    interface Inner{
        void handler();
    }

    public void registerCallback(Inner inner){
        inner.handler();
    }


    public static void main(String[] args){
        NestedInterface nestedInterface = new NestedInterface();
        nestedInterface.registerCallback(new Inner() {
            public void handler() {
                System.out.println("");
            }
        });
    }
}
```
- 也提供了一些封装特性，就像是一个命名空间的作用


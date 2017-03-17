---
title: 算法学习-最小可用ID
tags: [算法学习]
toc: true
---

# 介绍
《算法新解》的学习笔记。
# 题目描述
>假设我们使⽤⾮负整数作为某个系统的的ID，所有⽤户都由⼀个ID唯⼀确定。任何时间，这个系统中有些ID处在使⽤中的状态，有些ID则可以⽤于分配给新⽤户。现在的问题是，怎样才能找到最⼩的可分配ID呢？

>[18, 4, 8, 9, 16, 1, 14, 7, 19, 3, 0, 5, 2, 11, 6]

# 解决
## 普通解法
普通解法（暴力破解法），描述如下（data表示数组集合）：   
对num=1开始的数据进行循环，判断num是否在data中，如果在，num++，继续循环；不在，则返回num的值。   
```
    /**
     * 普通算法
     * 对num=1开始的数据进行循环，判断num是否在data中，
     * 如果在，num++，继续循环；
     * 不在，则返回num的值。
     * @return
     */
    public static int ordinaryAlgorithms(){
        int num = 1;
        while (true){
           boolean hasFlag = false;
           for(int i = 0; i < data.length; i++){
               if(data[i] == num){
                   hasFlag = true;
                   break;
               }
           }
           if(!hasFlag){
               break;
           }else{
               num ++;
           }
        }
        return num;
    }
```



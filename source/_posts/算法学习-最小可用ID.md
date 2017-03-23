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

## 改进算法
考虑到以下事实：   
对于[x1,x2,...,xn]n个非负整数data
- 如果为[0,n-1]，那么最小可用数为n 
- 如果数组中有任意一个数大于n，那么相应的最小可用数必然小于n
则最小可用数`result <= n`。   
这样，我们设定一个boolean flag[n+1]，默认值全为false。   
遍历data，对于每个元素x，如果`x<n`，那么我们可以在flag中找到对应的flag[x]，设置`flag[x] = true`，表明data数组中，存在x。   
最终，得到flag[]数组，在data中存在的数据x对应flag[x]已经为true，只需要找到最小的`flag[n]==false`即可。
```
 /**
     * 改进算法
     * 基于以下事实：
     * 如果[x1,x2,...,xn]n个非负整数data，如果为[0,n-1]，那么最小可用数为n；如果数组中有任意一个数大于n，那么相应的最小可用数必然小于n，即
     * result <= n
     * 这样，定义一个n+1长度的布尔值元素flag[]，默认值均为false；
     * 遍历data，如果data[i] < n，那么对应的flag[data[i]]=true
     * 最后遍历flag[]，找到第一个false的位置index，index即为所求的
     * 最后，如果data为[0,1,2,...,n-1]，那么只有flag[n]为false，最终的值也就为n
     * @return
     */
    public static int improvement_one(){
        boolean[] flag = new boolean[data.length+1];
        for(int i = 0; i < flag.length; i++){
            flag[i] = false;
        }
        for(int i = 0; i < data.length; i++){
            int dataEle = data[i];
            if(dataEle < data.length){
                flag[dataEle] = true;
            }
        }

        for(int i = 0; i < flag.length; i++){
            if(flag[i] == false){
                return i;
            }
        }
        throw new RuntimeException("没有找到数据");
    }
```




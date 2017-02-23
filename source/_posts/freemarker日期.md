---
title: Freemarker 日期处理
tags: [Freemarker,日期]
toc: true
---

## 背景
在freemarker页面，根据日期大小，来判断是否显示日期内容。
## 分析
- 输入的数据可能格式错误，需要进行正则匹配
- 将字符串转变成日期对象
- 日期大小比较   

## 实现
### 正则处理
首先约定，处理的日期数据均为 "yyyy-mm-dd"格式，不符合上述内容不予显示。
在freemarker里，实现正则匹配，需要利用freemarker的内置函数matches。
```
<#assign reg="^\\d{4}(\\-)\\d{2}\\1\\d{2}$" />
<#if variable?? && variable.matches(reg)><!-- variable存在并且与reg进行匹配 -->
</#if>
```

### 字符串和日期转换
#### 字符串转成日期
如果匹配相应的格式，这里需要将字符串数据转变成日期数据。

```
${variable?date("yyyy-MM-dd")}
```

#### 日期转成字符串
当然，在接受到java.util.date类型，如果在Freemarker显示，则需要利用日期处理函数。
```
${parameters.fieldDate?date}                                           //标准日期转日期字符串
${parameters.fieldDate?datetime}　　　　　　　　　　　　　//标准日期转日期+时间 字符串
${parameters.fieldDate?string("yyyy-MM-dd HH:mm:ss")}   //标准日期转自定格式 字符串
${(data.validateTimeDate?string("yyyy-MM-dd"))}               //yyyy-MM-dd日期格式的字符串
```

### 大小比较
大小比较就很简单了，转变好日期格式后，即可以进行比较。
```
<#assign startDate="1970-01-01"?date("yyyy-MM-dd") /> <!-- 声明日期对象 -->
<#if variable?date("yyyy-MM-dd") gt startDate>
			${variable}
</#if>
```

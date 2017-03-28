---
title: 工具使用-IDEA
tags: [工具使用,IDEA]
toc: true
---

# 自定义live template
## logger
快速生成logger，在File->Setting->Editor->Live Templates中，选择JAVA，点击右侧绿色加号添加，内容如下：
- Abbreviation:<log>
- Desciption:日志生成
- Template text
```
/** logger */
private static final Logger logger = LoggerFactory.getLogger($CLASS$.class);
```

同时，点击Edit variables按钮，对于上述代码中的`$CLASS$`变量指定值，`Expression`中选取`className()`，同时勾选`Skip if defined`。   
当前会提示No applicable contexts。点击`Define`，勾选`EveryWhere`下的`JAVA`中的相应内容。
这样，在代码中输入`<log>`后，按`Tab`就可以快速生成当前类的日志，当然还需要自行引包。

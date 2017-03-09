---
title: 项目搭建Demo[dubbo-spring-mybatis-freemarker]（二）
tags: [项目实践]
toc: true
---
# mybatis





# 总结
搭建过程中，遇到一些问题，总结如下。
## IDEA新建子项目问题
`pom.xml already exists in vfs`，原因是在建立是Content root和Module file location的配置不正确，选择了父项目的目录，当前父项目中已经有了pom文件，所以会提示错误。

## tomcat部署时ClassNotFound
是因为在部署的时候没有部署maven依赖的包，Project Structure->Artifacts->Output Layout，将Available Elements下的maven依赖，以及dubbo-service-api依赖添加到部署目录的lib包下。
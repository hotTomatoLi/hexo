---
title:  一次maven的冲突解决
tags: [maven,冲突]
toc: true
---

## 背景
当前项目有统一的公共父pom文件，为的是保证各个项目组的依赖统一。但是，当前统一的配置与自己的项目有冲突，因此，对冲突进行了排解。
主要目的是升级现有的spring框架，需要检查当前项目的依赖关系。
## maven命令

### mvn dependency tree
`mvn dependency:tree`命令可以查看当前项目的依赖关系。可以将当前POM的依赖关系按照树形结构展开。
```
[INFO] +- org.springframework:spring-context:jar:4.2.8.RELEASE:compile
[INFO] |  +- org.springframework:spring-aop:jar:4.2.8.RELEASE:compile
[INFO] |  |  \- aopalliance:aopalliance:jar:1.0:compile
[INFO] |  +- org.springframework:spring-beans:jar:4.2.8.RELEASE:compile
[INFO] |  \- org.springframework:spring-expression:jar:4.2.8.RELEASE:compile
[INFO] +- org.springframework:spring-core:jar:4.2.8.RELEASE:compile
[INFO] +- org.springframework:spring-context-support:jar:4.2.8.RELEASE:compile
[INFO] +- org.springframework:spring-jdbc:jar:4.2.8.RELEASE:compile
[INFO] +- org.springframework:spring-tx:jar:4.2.8.RELEASE:compile
[INFO] +- org.springframework:spring-aspects:jar:4.2.8.RELEASE:compile
[INFO] |  \- org.aspectj:aspectjweaver:jar:1.8.9:compile
[INFO] +- org.freemarker:freemarker:jar:2.3.20:compile
[INFO] +- mysql:mysql-connector-java:jar:5.1.22:compile
[INFO] +- c3p0:c3p0:jar:0.9.1.2:compile
[INFO] +- org.slf4j:slf4j-api:jar:1.7.5:compile
[INFO] +- org.slf4j:jcl-over-slf4j:jar:1.7.5:compile
[INFO] +- org.slf4j:log4j-over-slf4j:jar:1.7.5:compile
[INFO] +- ch.qos.logback:logback-classic:jar:1.0.13:compile
[INFO] |  \- ch.qos.logback:logback-core:jar:1.0.13:compile
```
上述树形结构里，’\-’表示当前父节点最后的子节点。

#### –Dverbose
加上这个参数，可以将当前所有的依赖关系都展示出来，包括来自不同处的依赖项。
```
[INFO] +- org.springframework:spring-context-support:jar:4.2.8.RELEASE:compile
[INFO] |  +- (org.springframework:spring-beans:jar:4.2.8.RELEASE:compile - omitted for duplicate)
[INFO] |  +- (org.springframework:spring-context:jar:4.1.1.RELEASE:compile - version managed from 4.2.8.RELEASE; omitted for conflict with 4.2.8.RELEASE)
[INFO] |  \- (org.springframework:spring-core:jar:4.1.1.RELEASE:compile - version managed from 4.2.8.RELEASE; omitted for conflict with 4.2.8.RELEASE)
[INFO] +- org.springframework:spring-jdbc:jar:4.2.8.RELEASE:compile
[INFO] |  +- (org.springframework:spring-beans:jar:4.2.8.RELEASE:compile - omitted for duplicate)
[INFO] |  +- (org.springframework:spring-core:jar:4.1.1.RELEASE:compile - version managed from 4.2.8.RELEASE; omitted for conflict with 4.2.8.RELEASE)
[INFO] |  \- (org.springframework:spring-tx:jar:4.1.1.RELEASE:compile - version managed from 4.2.8.RELEASE; omitted for conflict with 4.2.8.RELEASE)
[INFO] +- org.springframework:spring-tx:jar:4.2.8.RELEASE:compile
[INFO] |  +- (org.springframework:spring-beans:jar:4.2.8.RELEASE:compile - omitted for duplicate)
[INFO] |  \- (org.springframework:spring-core:jar:4.1.1.RELEASE:compile - version managed from 4.2.8.RELEASE; omitted for conflict with 4.2.8.RELEASE)
[INFO] +- org.springframework:spring-aspects:jar:4.2.8.RELEASE:compile
[INFO] |  \- org.aspectj:aspectjweaver:jar:1.8.9:compile
```
#### –Dincludes
`-Dincludes` 可以进行参数过滤，如果需要查询spring相关的依赖，可以
`mvn dependency:tree -Dverbose -Dincludes=*spring*:*spring*`，结果
```
[INFO] +- org.springframework:spring-context:jar:4.2.8.RELEASE:compile
[INFO] |  +- org.springframework:spring-aop:jar:4.2.8.RELEASE:compile
[INFO] |  |  +- (org.springframework:spring-beans:jar:4.2.8.RELEASE:compile - omitted for duplicate)
[INFO] |  |  \- (org.springframework:spring-core:jar:4.1.1.RELEASE:compile - version managed from 4.2.8.RELEASE; omitted for duplicate)
[INFO] |  +- org.springframework:spring-beans:jar:4.2.8.RELEASE:compile
[INFO] |  |  \- (org.springframework:spring-core:jar:4.1.1.RELEASE:compile - version managed from 4.2.8.RELEASE; omitted for duplicate)
[INFO] |  +- (org.springframework:spring-core:jar:4.1.1.RELEASE:compile - version managed from 4.2.8.RELEASE; omitted for duplicate)
[INFO] |  \- org.springframework:spring-expression:jar:4.2.8.RELEASE:compile
[INFO] |     \- (org.springframework:spring-core:jar:4.1.1.RELEASE:compile - version managed from 4.2.8.RELEASE; omitted for duplicate)
[INFO] +- org.springframework:spring-core:jar:4.2.8.RELEASE:compile
[INFO] +- org.springframework:spring-context-support:jar:4.2.8.RELEASE:compile
[INFO] |  +- (org.springframework:spring-beans:jar:4.2.8.RELEASE:compile - omitted for duplicate)
[INFO] |  +- (org.springframework:spring-context:jar:4.1.1.RELEASE:compile - version managed from 4.2.8.RELEASE; omitted for conflict with 4.2.8.RELEASE)
[INFO] |  \- (org.springframework:spring-core:jar:4.1.1.RELEASE:compile - version managed from 4.2.8.RELEASE; omitted for conflict with 4.2.8.RELEASE)
[INFO] +- org.springframework:spring-jdbc:jar:4.2.8.RELEASE:compile
[INFO] |  +- (org.springframework:spring-beans:jar:4.2.8.RELEASE:compile - omitted for duplicate)
[INFO] |  +- (org.springframework:spring-core:jar:4.1.1.RELEASE:compile - version managed from 4.2.8.RELEASE; omitted for conflict with 4.2.8.RELEASE)
[INFO] |  \- (org.springframework:spring-tx:jar:4.1.1.RELEASE:compile - version managed from 4.2.8.RELEASE; omitted for conflict with 4.2.8.RELEASE)
[INFO] +- org.springframework:spring-tx:jar:4.2.8.RELEASE:compile
[INFO] |  +- (org.springframework:spring-beans:jar:4.2.8.RELEASE:compile - omitted for duplicate)
[INFO] |  \- (org.springframework:spring-core:jar:4.1.1.RELEASE:compile - version managed from 4.2.8.RELEASE; omitted for conflict with 4.2.8.RELEASE)
[INFO] +- org.springframework:spring-aspects:jar:4.2.8.RELEASE:compile
[INFO] +- org.springframework:spring-test:jar:4.2.8.RELEASE:test
[INFO] |  \- (org.springframework:spring-core:jar:4.1.1.RELEASE:test - version managed from 4.2.8.RELEASE; omitted for conflict with 4.2.8.RELEASE)
```
当然，可以将输出结果导入到文本中。只需要在命令后加入”>a.txt”即可。

###	 mvn help:effective-pom
上述命令可以将当前项目自己的POM已经从父类中继承的POM内容输出，可以输出到文本中
`mvn help:effective-pom>a.txt`

### mvn dependency:analyze
`dependency:analyze`是通过分析bytecode来输出报告。上面的输出有两部分，一是'Used undeclared dependencies'（使用但未定义的依赖项），
二是'Unused declared dependencies'（未使用但却定义的依赖项）。   
'Used undeclared dependencies'   
该列表列出的是程序代码直接用到的、但并没在pom.xml里定义的依赖项。   
Unused declared dependencies   
该列表列出的是程序代码没用到的、但在pom.xml里定义的依赖项。
注意，该列表可能不准确，因为程序代码可能使用了反射技术，在运行时需要这些jar包存在。

## 排除依赖

### dependency
在dependency中，可以通过exclusions标签进行依赖排除。
排除spring
```
<exclusions>
   <exclusion>
      <groupId>org.springframework</groupId>
      <artifactId>spring</artifactId>
   </exclusion>
</exclusions>
```
排除所有
```
<exclusions>
   <exclusion>
      <groupId>*</groupId>
      <artifactId>*</artifactId>
   </exclusion>
</exclusions>
```
### 继承
当有的引入是由当前项目POM继承父POM时，子类继承了父类的dependencyManagement。这种机制本身作用是将公共模块提取出来，
在子类中可以不用声明版本号，统一按照父类的定义。   
但是，如果我们想要指定新的版本号，则需要在子类的dependency中声明版本号，只有dependencies元素中没有指明版本信息时，
dependencyManagement 中的 dependencies 元素才起作用。   
可以通过mvn help:effective-pom查看生成的POM，可以发现子类中包含了dependencyManagement元素。
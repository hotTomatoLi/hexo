---
title: ELE_SHOP开发
tags: [JAVA,Spring MVC,Freemarker,Mybatis]
toc: true
---
## 背景
记录ELE_SHOP开发过程

## 技术选型
Spring 4.2.8.RELEASE
Freemarker 2.3.23
mysql 5.7.16
Mybatis 3.4.1

## 开发过程记录
### 基本环境搭建
#### 新建MAVEN工程
- POM配置
```
<?xml version="1.0" encoding="UTF-8"?>
 <project xmlns="http://maven.apache.org/POM/4.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
     <modelVersion>4.0.0</modelVersion>
 
     <groupId>com.leegebe.web</groupId>
     <artifactId>ele-shop</artifactId>
     <version>0.0.1-SNAPSHOT</version>
 </project>
```
- .gitignore配置
```
#Maven#
target/

#IDEA#
.idea/
*.iml

#Eclipse#
.settings/
.metadata/
.classpath
.project
Servers
```

#### 引入Spring+Freemarker
- 引入pom文件
```
    <properties>
        <spring.version>4.2.8.RELEASE</spring.version>
        <log4j.version>1.2.14</log4j.version>
        <slf4j.version>1.7.5</slf4j.version>
        <freemarker.version>2.3.23</freemarker.version>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-oxm</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-tx</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aop</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context-support</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aop</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>${log4j.version}</version>
        </dependency>
        <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-api</artifactId>
        <version>${slf4j.version}</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>${slf4j.version}</version>
        </dependency>
        <dependency>
            <groupId>org.freemarker</groupId>
            <artifactId>freemarker</artifactId>
            <version>${freemarker.version}</version>
        </dependency>

    </dependencies>
```
- 引入Spring配置
springmvc.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:util="http://www.springframework.org/schema/util"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context.xsd
http://www.springframework.org/schema/util
http://www.springframework.org/schema/util/spring-util.xsd
http://www.springframework.org/schema/mvc
http://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!-- 添加注解驱动 -->
    <mvc:annotation-driven />
    <!-- 默认扫描的包路径 -->
    <context:component-scan base-package="com.leegebe.controller" />

    <!-- FreeMarkerConfigurer -->
    <bean id="freemarkerConfig"
          class="org.springframework.web.servlet.view.freemarker.FreeMarkerConfigurer">
        <property name="templateLoaderPath">
            <value>/WEB-INF/ftl/</value>
        </property>
        <property name="freemarkerSettings">
            <!-- 设置FreeMarker环境属性 -->
            <props>
                <prop key="template_update_delay">5</prop><!--刷新模板的周  期，单位为秒 -->
                <prop key="default_encoding">UTF-8</prop><!--模板的编码格式 -->
                <prop key="locale">UTF-8</prop><!-- 本地化设置 -->
                <prop key="datetime_format">yyyy-MM-dd HH:mm:ss</prop>
                <prop key="time_format">HH:mm:ss</prop>
                <prop key="number_format">0.####</prop>
                <prop key="boolean_format">true,false</prop>
                <prop key="whitespace_stripping">true</prop>
                <prop key="tag_syntax">auto_detect</prop>
                <prop key="url_escaping_charset">UTF-8</prop>
            </props>
        </property>
    </bean>

    <bean id="freemarkerResolver"
          class="org.springframework.web.servlet.view.freemarker.FreeMarkerViewResolver">
        <property name="order" value="1" />
        <property name="suffix" value=".ftl" />
        <property name="contentType" value="text/html;charset=utf-8" />
        <property name="viewClass">
            <value>org.springframework.web.servlet.view.freemarker.FreeMarkerView
            </value>
        </property>
    </bean>
</beans>
```
配置web.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">
    <servlet>
        <servlet-name>dispatcher</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet
        </servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <!-- 设置dispatchservlet的匹配模式，通过把dispatchservlet映射到/，默认servlet会处理所有的请求，包括静态资源 -->
    <servlet-mapping>
        <servlet-name>dispatcher</servlet-name>
        <url-pattern>*.html</url-pattern>
    </servlet-mapping>
</web-app>
```
- 测试
IndexController类
```
package com.leegebe.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.RequestMapping;

/**
 * Index控制器
 * @author leegebe
 * @since 1.0.0
 *
 */
@Controller
public class IndexController {

    @RequestMapping("index.html")
    public String index(ModelMap modelMap){
        modelMap.put("title","名称");
        return "index";
    }
}

```
测试页面
```
<!DOCTYPE html>
<html>
<head>
    <title>${title}</title>
</head>
<body>
${title}
</body>
</html>
```

#### 引入Spring+mysql+Mybatis
- 增加mysql(5.1.40) 和mybatis(3.4.1)和mybatsi-spring(1.3.0)的maven配置
- 增加spring.xml配置，配置了需要扫描的注解包，sqlSessionFactory，transactionManager，datasource

#### 引入mybatis-generator
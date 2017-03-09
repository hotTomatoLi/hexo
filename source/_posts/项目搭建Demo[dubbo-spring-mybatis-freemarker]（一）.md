---
title: 项目搭建Demo[dubbo-spring-mybatis-freemarker]（一）
tags: [项目实践]
toc: true
---

# 说明
搭建一个示例项目，由dubbo、spring、mybatis和freemarker基本框架组成。主要目的是根据当前工作中使用的技术，搭建一个可以快速开发的框架，也方便自己使用。使用idea进行搭建。
- service项目，后端服务，包括一个api项目和一个impl项目，api项目主要是前后端共享的jar包，impl项目是实现具体功能；
- app项目，前端程序，运行在tomcat中

# service搭建过程
## 建立Service项目及其子项目
- 建立dubbo-service的maven项目，删除src目录，只保留pom文件，作为父模块；
- 建立子项目dubbo-service-api和dubbo-service-impl（需要明确指定子项目的Content root和Module file location时，否则会报错:`pom.xml already exists in vfs`）

## 增加依赖包
### dubbo-service
增加依赖包是在dubbo-service的pom里，增加`dependency management`，从而可以控制子模块的依赖的版本。   
增加的依赖包括：
- dubbo相关依赖，需要移除dubbo的spring，版本太低，同时也移除了日志包；
- Spring相关依赖
- 日志相关依赖，slf4j和logback
- MyBatis相关依赖
- 数据库（mysql）、连接池相关依赖
- Commons 相关
- Quartz相关
- zookeeper相关

### dubbo-service-api
由于本身就是接口模块，暂时不需要增加依赖关系。

### dubbo-service-impl
基本上按照dubbo-service中的dependency management中的引入，只是无需标明版本号。

## 增加dubbo配置文件和日志文件
### dubbo.propertise
系统会自动加载ClassPath下的dubbo.properties，因此，在test的resources下增加dubbo.properties。（官方推荐采用xml配置方式）
```
dubbo.container=spring
dubbo.application.name=demo
dubbo.application.owner=com.leegebe
dubbo.registry.register=true
# 需要替换成自己的zookeeper
dubbo.registry.address=zookeeper://192.168.255.255:2181
dubbo.registry.file=dubbo.registry.properties

# 传输数据大小，默认值为8M
dubbo.protocol.payload=30000000 
```

### 日志文件
采用的是logback日志
```
<?xml version="1.0" encoding="UTF-8" ?>
<configuration scan="true" scanPeriod="60 seconds" debug="false">
    <!-- 控制台 -->
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger - %msg%n</pattern>
        </encoder>
    </appender>

    <root level="DEBUG">
    	<appender-ref ref="STDOUT"/>
    </root>

</configuration>
```

## 增加spring配置文件
下面的配置放在了resources/META-INF/spring目录，dubbo会自动加载META-INF/spring目录下的所有Spring配置，可以合并成一个，为了表示方便进行拆分。
### spring-bean.xml
配置所有的bean。
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
      http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
</beans>
```
### provider.xml
用于定义提供的dubbo服务。
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
               http://www.springframework.org/schema/beans/spring-beans.xsd
                       http://code.alibabatech.com/schema/dubbo
                               http://code.alibabatech.com/schema/dubbo/dubbo.xsd">
</beans>
```

## 编写接口和实现
### 接口
在dubbo-service-api中编写测试接口。
```
/**
 * dubbo的示例Service
 */
public interface DemoService {

    String sayHello(String name);

}
```
### 实现
在dubbo-service-impl中编写相应的实现类.
```
/**
 * demoService的实现类
 */
public class DemoServiceImpl implements DemoService {

    public String sayHello(String name) {
        return "Hello" + name;
    }
}
```

### 配置spring的Bean
在spring.xml中，增加bean。
```
<bean id="demoService" class="com.leegebe.dubbo.DemoServiceImpl" />
```

### 配置dubbo服务
在provider.xml中，增加dubbo服务。
```
<dubbo:service interface="com.leegebe.dubbo.DemoService" ref="demoService" />
```

## 启动服务
在test包下，编写dubbo启动类的方法。
```
/**
 * Dubbo主程序
 */
public class DubboApplication {

    public static void main(String[] args) {
        Main.main(args);
    }

}
```
启动后，可以通过dubbo-admin查找到相应的服务。


# app端项目搭建
## 建立app项目
新建maven项目

## 增加依赖
- spring，spring-web依赖
- dubbo依赖
- 日志依赖
- freemarker依赖
- servlet依赖

## 增加web的Module
需要定义Web Rersource Directory目录。

## 配置web.xml
指定DispatcherServlet的配置文件springmvc.xml，和spring的配置目录/spring/*.xml。
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

    <!-- spring config -->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>
            classpath:/spring/*.xml,
        </param-value>
    </context-param>

    <listener>
        <listener-class>
            org.springframework.web.context.ContextLoaderListener
        </listener-class>
    </listener>
    <context-param>
        <param-name>log4jConfigLocation</param-name>
        <param-value>WEB-INF/log4j.properties</param-value>
    </context-param>

    <listener>
        <listener-class>
            org.springframework.web.util.Log4jConfigListener
        </listener-class>
    </listener>
</web-app>
```

## 配置springmvc.xml
定义了扫描的包，并配置了freemarker相关内容。
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

## 配置ftl文件
新增WEB-INF/ftl/index.ftl文件，作为展示页面。

## 配置dubbo相关
### dubbo.properties
在resources下增加dubbo配置。
```
##
# Dubbo Configuration
##
dubbo.container=spring
dubbo.application.name=demo
dubbo.application.owner=com.leegebe
dubbo.registry.register=true
dubbo.registry.address=zookeeper://192.168.255.255:2181
dubbo.protocol.dubbo.port=20958
dubbo.registry.file=dubbo.registry.properties
```

### consumer.xml
在consumer.xml文件中，编写dubbo服务端提供的服务。
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
       xsi:schemaLocation="http://www.springframework.org/schema/beans        http://www.springframework.org/schema/beans/spring-beans.xsd        http://code.alibabatech.com/schema/dubbo        http://code.alibabatech.com/schema/dubbo/dubbo.xsd">


    <!-- 生成远程服务代理，可以和本地bean一样使用demoService -->
    <dubbo:reference id="demoService" interface="com.leegebe.dubbo.DemoService" />

</beans>
```

## controller类
```
@Controller
public class IndexController {

    @Resource
    DemoService demoService;

    @RequestMapping("index.html")
    public String index(ModelMap modelMap){
        modelMap.put("title",demoService.sayHello("index"));
        return "index";
    }
}
```
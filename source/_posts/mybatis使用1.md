---
title:  Mybatis使用（一）
tags: [Mybatis]
toc: true
---
# Mybatis
Mybatis是一个ORM框架，目的是减少繁琐的数据库连接代码的编写，同时将SQL语句从代码中分离出来。其中，核心的几个类为
- SqlSessionFactory
- SqlSession
- XXXMapper.java（或XXXDao.java），均为接口
- XXXmapper.xml

大概描述一下Mybatis的流程
- 编写mapper.xml文件，编写sql语句，指定关联的mapper.java
- 建立SqlSessionFactory（加载配置文件，配置文件里指定mapper.xml的位置）
- SqlSessionFactory生成SqlSession
- SqlSession生成Mapper.java的实例，执行对应的方法，从而执行相应的sql脚本

# 搭建
## 依赖
在Maven项目中，引入对应的jar。当然，也可以替换成源码模式，只需要将Mybatis源码中依赖引入即可。
```
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.4.2</version>
        </dependency>
```

## 数据库配置
配置数据库文件conf.properties
```
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/leegebe
username=root
password=XXX
```
## mybatis配置文件
配置mybatis的数据库连接、mapper文件等等。
```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <properties resource="conf.properties"/>

    <typeAliases>
        <typeAlias type="com.leegebe.mybatis.model.User" alias="user" />
    </typeAliases>

    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>

    <mappers>
        <mapper resource="mappers/UserMapper.xml"/>
    </mappers>


</configuration>
```

## mapper.xml
编写UserMapper.xml文件，在其中编写实际的sql。
```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.leegebe.mybatis.mapper.UserMapper">
    <resultMap id="baseResultMap" type="user">
        <id column="USER_ID" property="userId" javaType="String" jdbcType="VARCHAR" />
        <result column="ID" property="id" javaType="int" jdbcType="INTEGER" />
        <result column="USER_NAME" property="userName" javaType="String" jdbcType="VARCHAR" />
        <result column="LOGIN_NAME" property="loginName" javaType="String" jdbcType="VARCHAR" />
        <result column="PASSWORD" property="password" javaType="String" jdbcType="VARCHAR" />
        <result column="CREATE_TIME" property="createTime" javaType="long" jdbcType="BIGINT" />
        <result column="UPDATE_TIME" property="updateTime" javaType="long" jdbcType="BIGINT" />
    </resultMap>

    <select id="selectUser" resultMap="baseResultMap">
        select * from USER where USER_ID = #{userId}
    </select>

    <insert id="insertUser">
        INSERT INTO USER(USER_ID,USER_NAME,LOGIN_NAME,PASSWORD,CREATE_TIME,UPDATE_TIME)
        VALUES(#{userId},#{userName},#{loginName},#{password},#{createTime},#{updateTime})
    </insert>
</mapper>
```
## 数据模型User
```
public class User {

    private int id;

    private String userId;

    private String userName;

    private String loginName;

    private String password;

    private long createTime;

    private long updateTime;

    private int validated;

    ...
}
```

## Mapper.java
编写UserMapper接口，方法名与UserMapper.xml中的id一致。
```
/**
 * User对象的数据接口
 */
public interface UserMapper {

    /**
     * 根据userId查询User对象
     * @param userId
     * @return
     */
    User selectUser(String userId);

    /**
     * 新增一个用户对象
     * @param user
     * @return
     */
    Integer insertUser(User user);

}
```

## SqlSessionFactoryManager
用于提供SqlSessionFactory的类。
```
public class SqlSessionFactoryManager {

    private static SqlSessionFactory factory = null;

    private static String fileName = "mybatis-config.xml";

    private SqlSessionFactoryManager(){}

    public static void initMapper(String sqlMapperFileName){
        fileName = sqlMapperFileName;
    }

    public static SqlSessionFactory getFactory() {

        try {
            if(factory == null) {
                Reader reader = Resources.getResourceAsReader(fileName);
                SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
                factory = builder.build(reader);
                builder = null;
            }
        } catch (IOException e) {
            e.printStackTrace();
            return null;
        }

        return factory;
    }
```

## Service中调用
```
public class UserService {

    UserMapper userMapper;

    /**
     * 新增User对象
     * @param user
     * @return
     */
    public User addUser(User user){
        SqlSessionFactory factory = SqlSessionFactoryManager.getFactory();
        SqlSession sqlSession = factory.openSession(true);
        userMapper = sqlSession.getMapper(UserMapper.class);
        userMapper.insertUser(user);
        return user;
    }

    /**
     * 根据UserId查询一个对象
     * @param userId
     * @return
     */
    public User selectUser(String userId){
        SqlSessionFactory factory = SqlSessionFactoryManager.getFactory();
        SqlSession sqlSession = factory.openSession(true);
        userMapper = sqlSession.getMapper(UserMapper.class);
        return userMapper.selectUser(userId);
    }
}
```





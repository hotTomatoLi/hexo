---
title:  Mybatis 使用
tags: [Mybatis]
toc: true
---
## Mybatis





## 使用问题
### 查询结果集为队列，返回值只有最后一条
这是在Mybatis版本为3.2.1里发现的。具体情境如下：
在mapper配置里，有如下的数据配置
```
    <resultMap id="map1" type="cn.com.XXX" >
        <result column="T1_ID" property="id" jdbcType="INTEGER"/>
         <result column="XX" property="XX" jdbcType="INTEGER"/>
    </resultMap>
    <resultMap id="map2" type="cn.com.XXX" >
        <result column="XX" property="XX" jdbcType="BIGINT" />
        <result column="XX" property="XX" jdbcType="BIGINT" />
    </resultMap>
    <resultMap id="BaseResultMap" type="cn.com.XXX" >
        <association property="XX"  javaType="cn.com.XXX" resultMap="map1"/>
        <association property="XX"  javaType="cn.com.XXX" resultMap="map2"/>
    </resultMap>
```
在返回值中（调用page分页查询，数据集结果大于1），发现最终的List中只有一条记录，和SQL单独执行时的结果集相比，改记录为最后一条记录。于是跟踪代码，直到`FastResultSetHandler.handleRowValues`中，发现被
`NestedResultSetHandler`类覆盖，而在`NestedResultSetHandler`类中，
```
 protected void handleRowValues(ResultSet rs, ResultMap resultMap, ResultHandler resultHandler, RowBounds rowBounds, ResultColumnCache resultColumnCache) throws SQLException {
    final DefaultResultContext resultContext = new DefaultResultContext();
    skipRows(rs, rowBounds);
    Object rowValue = null;
    while (shouldProcessMoreRows(rs, resultContext, rowBounds)) {
      final ResultMap discriminatedResultMap = resolveDiscriminatedResultMap(rs, resultMap, null);
      final CacheKey rowKey = createRowKey(discriminatedResultMap, rs, null, resultColumnCache);
      Object partialObject = objectCache.get(rowKey);
      if (partialObject == null && rowValue != null) { // issue #542 delay calling ResultHandler until object ends
        if (mappedStatement.isResultOrdered()) objectCache.clear(); // issue #577 clear memory if ordered
        callResultHandler(resultHandler, resultContext, rowValue);
      } 
      rowValue = getRowValue(rs, discriminatedResultMap, rowKey, rowKey, null, resultColumnCache, partialObject);
    }
    if (rowValue != null) callResultHandler(resultHandler, resultContext, rowValue);
  }
```
发现，rowValue被不断地覆盖。

具体bug[这里](https://github.com/mybatis/mybatis-3/issues/39)
如果增加了一个id，则返回正常。
```
    <resultMap id="map1" type="cn.com.XXX" >
        <id column="ID" property="id" jdbcType="INTEGER"/>
        <result column="T1_ID" property="id" jdbcType="INTEGER"/>
         <result column="XX" property="XX" jdbcType="INTEGER"/>
    </resultMap>
```


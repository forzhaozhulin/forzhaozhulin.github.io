---
layout: post
title:  "Spring学习笔记---其它"
date:   2016-05-24 16:39:00
categories: JavaWeb
tags: Spring
author: 薛彬
---

* content
{:toc}





### <context:annotation-config/>

这个是隐式地向Spring容器注册BeanPostProcessor的配置，主要是用来使用@Resource、@PostConstruct、@PreDestroy、@Required等注解。

经常会和`<context:component-scan base-package=""/>`一起使用，这个是用来配置扫描包路径选项。

### <context:property-placeholder location="classpath:jdbc.properties"/>

这个是连接数据的配置。

location就是参数文件的位置。

这个参数文件的格式就是采取`name=value`键值对的形式，比如：

```xml
jdbc.driverClassName=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://127.0.0.1/data
jdbc.user=user
jdbc.password=123456
```

### C3P0数据源

标准的格式为：

```xml
<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource"          
        destroy-method="close">         
    <property name="driverClass" value=" oracle.jdbc.driver.OracleDriver "/>         
    <property name="jdbcUrl" value=" jdbc:oracle:thin:@localhost:1521:ora9i "/>         
    <property name="user" value="admin"/>         
    <property name="password" value="1234"/>         
</bean>   
```

如果配合上面那个一起用，可以写成这样：

```xml
<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource"          
        destroy-method="close">         
    <property name="driverClass" value="${jdbc.driverClassName}"/>         
    <property name="jdbcUrl" value="${jdbc.url}"/>         
    <property name="user" value="${jdbc.user}"/>         
    <property name="password" value="${jdbc.password}" />         
</bean>   
```

xml中可以通过`${var}`来获取值。

### 




---
layout: post
title:  "Spring学习笔记---Spring Security登录页"
date:   2016-06-21 14:19:00
categories: JavaWeb
tags: Spring
author: 薛彬
---

* content
{:toc}

本文是介绍了利用Spring Security来完成一个简单的登录页面。




![](http://i.imgur.com/eNkN4MD.png)


## 添加Spring Security命名空间

首先需要在Spring的配置文件中弄个添加Spring Security命名空间，考虑到可能会有很多不同的配置文件，我们将Spring Security的配置文件单独建立一个XML文件---`applicationContext-security.xml`，并在`applicationContext.xml`中加入如下语句:

```xml
<import resource="classpath:applicationContext-security.xml"/>
```

现在在`applicationContext-security.xml`中添加命名空间：

**applicationContext-security.xml**

```xml
<beans:beans xmlns="http://www.springframework.org/schema/security"
	xmlns:beans="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-4.2.xsd
    http://www.springframework.org/schema/security
    http://www.springframework.org/schema/security/spring-security-4.1.xsd">
</beans>
```

## 代理Servlet过滤器

在Spring Security中，是借助着一系列的Servlet过滤器来提供安全功能，所以就需要引入`<filter>`声明。在Spring中，我们只要简单的配置一个过滤器，就可以实现我们所需要的功能：

**web.xml**

```xml
<filter>
    <filter-name>springSecurityFilterChain</filter-name>
    <filter-class>
        org.springframework.web.filter.DelegatingFilterProxy
    </filter-class>
</filter>
<filter-mapping>
    <filter-name>springSecurityFilterChain</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

《Spring实战（第3版）》中是这样写的：

> DelegatingFilerProxy是一个特殊的Servlet过滤器，它将工作委托给一个javax.servlet.Filter实现类，这个实现类作为一个<bean>注册在Spring应用的上下文中。

> springSecurityFilterChain本身是一个特殊的过滤题，它可以链接任意一个或多个其它的过滤器，Spring Security依赖一系列Servlet过滤器来提供不同的安全特性。但是，你几乎不需要知道这些细节。 

也就是说，我们只需要配置一个这样简单的过滤器就可以使用Spring Security中的相应功能。

## 简单的登陆页面

我们只需要一个简单的jsp页面，就像这样：

```html
<%@ page contentType="text/html; charset=UTF-8" language="java"%>
<%@ taglib prefix="s" uri="http://www.springframework.org/tags"%>
<html>
<head>
<title></title>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
</head>
<body>
    <div class="container">
        <div class="login">
            <h1>用户登录</h1>
            <form method="POST" action="j_spring_security_check">
                <p><input type="text" name="j_username" value="" placeholder="请输入用户名"></p>
                <p><input type="password" name="j_password" placeholder="请输入密码"></p>
                <p><input class="btn btn-default" name="sumbit" type="submit" value="登陆"></p>
            </form>
        </div>
    </div>
</body>
</html>
```

注意其中的`form`表单，`j_spring_security_check`,`j_username`,`j_password`这些都是我们会用到的。

在`applicationContext-security.xml`中，需要这样配置：

```xml
<http auto-config="true" use-expressions="true">
    <headers>
        <frame-options policy="SAMEORIGIN" />
    </headers>
    <csrf disabled="true" />
    <form-login login-page="/login.jsp" default-target-url="/index.html" login-processing-url="/j_spring_security_check" authentication-failure-url="/login.jsp?error" username-parameter="j_username" password-parameter="j_password" />
    <intercept-url pattern="/login.jsp" access="permitAll()" />
</http>
```

登录前的页面是login.jsp，登录后的页面是index.html，如果账号密码错误则跳转到`login.jsp?error`,由于我们没设置就默认是跳到login.jsp了。


## 用户管理

**applicationContext-security.xml**

```xml
<authentication-manager>
    <authentication-provider>
        <user-service> 
            <user name="xb" password="hahaha" authorities="ROLE_USER,ROLE_ADMIN" /> 
            <user name="admin" password="123456" authorities="ROLE_ADMIN" /> 
        </user-service> 
    </authentication-provider>
</authentication-manager>
```

这样一个简单的登录页面就完成了。
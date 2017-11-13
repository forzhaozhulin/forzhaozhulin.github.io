---
layout: post
title:  "Spring学习笔记---图片加载失败"
date:   2016-06-01 16:39:00
categories: JavaWeb
tags: Spring
author: 薛彬
---

* content
{:toc}

问题：用spring mvc时无法访问图片，全是404





解决方法：web.xml下加上

```
<servlet-mapping>
    <servlet-name>default</servlet-name>
    <url-pattern>*.gif</url-pattern>
</servlet-mapping>
<servlet-mapping>
    <servlet-name>default</servlet-name>
    <url-pattern>*.jpg</url-pattern>
</servlet-mapping>
<servlet-mapping>
    <servlet-name>default</servlet-name>
    <url-pattern>*.png</url-pattern>
</servlet-mapping>
```

因为在上面配置spring的`dispatcherServlet`时，这样配置了：

```
<servlet-mapping>
    <servlet-name>springmvc-dispatcher</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
```

这样配置了之后导致所有的后缀都被spring拦截了，用上面的写法就可以使`*.jpg`/`*.png`等图片格式的不通过spring直接通过tomcat传到服务器上，就相当于静态页面访问了，`<servlet-name>default</servlet-name>`就是serlvet默认的写法。

> 要么把所有需要spring拦截的url统一定义后缀，这就是为什么很多例子里面用.do做servlet请求后缀的原因，这样<url-pattern>.do</url-pattern>就可以了

> any request->http server->spring DispatcherServlet->find a controller->process(if controller found)->return result to browser

> any request->http server->spring DispatcherServlet->find a controller->process(if controller does not exist)->return 404
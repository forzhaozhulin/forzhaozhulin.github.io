---
layout: post
title:  "JSP"
date:   2017-12-21
categories: web
tags: jsp 指令 九大内置对象
---

* content
{:toc}
## JSP

在初期，根据用户请求动态生成网页源代码的时候，我们使用的是response对象，获取输出流输出网页源代码，这种方式有几个问题：

1. 有大量的字符串拼接操作，操作复杂。
2. 前端工程师修改页面代码困难

为了解决以上问题sun公司给出了：Java Server Page——简称jsp技术

<!-- more -->

 Jsp是为了同时满足动态生成网页和简化页面书写的需求诞生的。

由于底层代码，jsp技术最终还是用java类，执行网页内容，jsp说到底还是一个Servlet.

### 在jsp中书写Java代码：

​    1、脚本声明

​    格式：<%!  书写Java代码  %>

​    2、脚本表达式

​    格式：<%= java代码 %>

​    可以将数据输出到页面

​    3、脚本片段

​    格式：<% Java代码片段 %>

### JSP中注释

1.  html注释：<!—注释 -->
2.  java注释：Java的注释必须嵌入在上面介绍的三个脚本中，不能在jsp其他位置书写。
3.  jsp自己的注释：<%-- 注释--%>

### jsp指令：

**什么是指令？**

就是一段代码。

指令是对jsp页面进行设置的。

指令格式：<%@ 指令的名字 key=value  key=value …….%>就是jsp指令

Key:属性名称

Value：属性值

Key和value值是用来设置jsp页面

#### Page指令

```jsp
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
```

jsp页面可设置的东西：

| language="java"                        | 声明当前jsp使用的编程语言，默认值是java(它现在也只支持java)（工作的时候也是写java） |
| -------------------------------------- | ---------------------------------------- |
| import="java.util.*"                   | 导入要使用的包（工作的时候，需要导入类的时候使用）                |
| pageEncoding="UTF-8"                   | 设置当前jsp源文件的编码表（工作的时候，就使用UTF-8）           |
| contentType="text/html; charset=utf-8" | 设置浏览器解析html的编码表，有pageEncoding的情况可以不设置。当设置过pageEncoding="UTF-8"之后，浏览器解析的时候，默认使用UTF-8，所以不再重新设置编码表。 |
| errorPage="500.jsp"                    | 设置在当前jsp页面(jsp3.jsp)发生异常(int I = 1/0;)后，跳转那个页面（500.jsp）。（工作的时候，如果页面有可能发生错误） |
| isELIgnored="true"                     | 是否解析jsp中的EL表达式（工作的时候，一般不写，使用默认的，默认为false解析El表达式） |
| session="true"                         | 设置在当前的页面中是否可以直接使用session对象（一般不设置，默认为true） |
| isErrorPage="true"                     | 设置当前的JSP页面(500.jsp)，是否是显示错误信息页面（500.jsp），如果是错误页面可以看到错误的信息（使用exception对象——jsp中的对象） |



一般开发的时候，会把整个项目中的常见的错误处理配置到web.xml文件中

测试统一错误配置的时候，需要将jsp page指令中 errorPage属性去掉。

```xml
<!-- 配置统一的错误页面  -->
  <error-page>	
  	<!-- 服务器的错误响应码 -->
  	<error-code>500</error-code>
  	<!-- 跳转那个页面 -->
  	<location>/500.jsp</location>
  </error-page>
```

#### include指令

引入其他的页面（头页面和尾），合并成一个页面，展示。这种引入方式称为静态引入。

```jsp
<%@include file="header.jsp" %><br>
这是新闻主体<br>
<%@include file="footer.jsp" %>
```

使用这个include指令三个jsp文件最终变成一个class文件

#### taglib指令

```jsp
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
```

作用：taglib指令在jsp导入已经定义好的标签库或者函数库（与类库（java对象，一个一个类）不是一个概念），方便程序员使用定义好的标签和函数。

在taglib指令中的属性：

uri：是当前标签对应的Java代码封装之后的名称空间——指定了一个网址，这个网址是用来确定，我们要引入的是那个标签库或者函数库

prefix：它是当前在页面上可以使用的标签的短名称——小名。

Taglib指令原理：本身jsp可以书写java代码，html、js、java在一个源文件上混杂一起维护不方便，所以将页面的java代码抽取成java类，然后将类配置到文件中，再在jsp页面上导入配置的java类，这样就可以以标签的方式使用定义好的java功能

### 9大内置对象

| HttpServletRequest request   | 请求                                       |
| ---------------------------- | ---------------------------------------- |
| HttpServletResponse response | 响应                                       |
| HttpSession session session  | 会话                                       |
| ServletContext application   | 表示当前项目对象                                 |
| ServletConfig config         | 专门获取当前这个Servlet的配置信息                     |
| Object page = this           | 它的表示是当前那个JSP页面对象。                        |
| PageContext pageContext      | 它表示的是当前jsp页面的上下文对象作用：它的主要功能就是可以获取到JSP页面上的其他八个内置对象。 |
| Throwable exception          | 主要是保存JSP页面上的异常信息的对象                      |
| JspWriter out                | 它相当于我们在Servlet中使用的response.getWriter     |



#### PageContext对象

pageConext它是一个工具类，有三个功能：

1. 获取jsp页面中的其他的8个内置对象
2. 给其他4个域对象==（容器）中的设置数据    
3. 从4个容器中取出数据

```html
    <%= pageContext.getException() %><br/>
    <%= pageContext.getOut() %><br/>
    <%= pageContext.getPage() %><br/>
    <%= pageContext.getRequest() %><br/>
            
    <%= pageContext.getResponse() %><br/>
    <%= pageContext.getServletConfig() %><br/>
    <%= pageContext.getServletContext() %><br/>
    <%= pageContext.getSession() %><br/>
		
存数据
<%
	pageContext.setAttribute("addr", "马尔代夫", pageContext.APPLICATION_SCOPE);
	pageContext.setAttribute("addr", "云南", pageContext.SESSION_SCOPE);
	pageContext.setAttribute("addr", "新加坡", pageContext.REQUEST_SCOPE);
	pageContext.setAttribute("addr", "东莞", pageContext.PAGE_SCOPE);

%>
	<%=pageContext.APPLICATION_SCOPE %><br>
	<%=pageContext.SESSION_SCOPE %><br>
	<%=pageContext.REQUEST_SCOPE %><br>
	<%=pageContext.PAGE_SCOPE %><br>
取数据
	<%=pageContext.getAttribute("addr", pageContext.APPLICATION_SCOPE) %><br>
	<%=pageContext.getAttribute("addr", pageContext.SESSION_SCOPE) %><br>
	<%=pageContext.getAttribute("addr", pageContext.REQUEST_SCOPE) %><br>
	<%=pageContext.getAttribute("addr", pageContext.PAGE_SCOPE) %><br>

```












































































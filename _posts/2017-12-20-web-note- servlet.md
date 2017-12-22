---
layout: post
title:  "servlet"
date:   2017-12-20
categories: web
tags: servlet
---

* content
{:toc}
# servlet

Servlet是Java Servlet的简称，称为小服务程序或服务连接器，用Java编写的服务器端程序，主要功能在于交互式地浏览和修改数据，生成动态Web内容。

<!-- more -->

Servlet是一个接口,处理请求和响应的java程序。

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171220/Ab1ldD22A4.png?imageslim)

### 创建servlet:

在实际我们一般不直接继承servlet,而是继承HttpServlet类，由他去继承servlet。

继承HttpServlet类，在web.xml中配置：

1、在.web包下 创建一个类 实现 HttpServlet 类

2、重写doGet和doPost方法

```java
public class DemoServlet extends HttpServlet{

	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		System.out.println("doGet....");
	}
	
	@Override
	protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		System.out.println("doPost....");

	}
}
```

3、在当前项目中的web.xml文件中配置当前开发好的Servlet程序

```xml
<servlet>
    <!-- servlet的名称 -->
  	<servlet-name>DemoServlet</servlet-name>
  	<!-- servlet的具体实现类 -->
  	<servlet-class>cn.itcast.web.DemoServlet</servlet-class>
  </servlet>
  <servlet-mapping>
  	<!-- servlet的名称 -->
  	<servlet-name>DemoServlet</servlet-name>
  	<!-- 访问这个servlet程序的请求路径 -->
  	<url-pattern>/DemoServlet</url-pattern>
  </servlet-mapping>
```

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171220/69GaB6bF8C.png?imageslim)总结：通过请求路径，查询web.xml中servlet配置，获取当前servlet对象，调用java程序。

###获取请求：

客户端通过表单提交信息

通过请求对象Request来获取请求参数

获取参数的方法：

getParameter(String name ）

​    根据name获取对应的值

 getParameterMap()

​    参数名作为key，参数值作为值，封装到map中

getParameterValues(String name)

​    获取name相同的所有value，如复选框

### 发送响应：

发送响应的对象response对象：

用于向浏览器发送响应使用getWriter方法

Servlet的生命周期：
当客户端体tomcat发送http请求访问servlet程序，tomcat首先会解析请求，检查内存中是否已经有了该servlet对象：

如果有直接使用对应的servlet对象;

如果没有就创建servlet实例对象，然后通过调用init() 方法实现Servlet的初始化工作。

需要注意的是，在整个servlet的生命周期内，init方法只被调用了一次。

当服务器关闭时servlet会随着Web应用的销毁而销毁，

销毁之前，tomcat会调用Servlet的destory方法，

以便让Servlet对象释放所占用的资源。



#### Web.xml中的路径书写方式：

1.全路径匹配

在书写url-pattern的时候，必须以/开始，后面书写具体浏览器访问时的路径。

配置：

```xml
<url-pattern>/DemoServlet</url-pattern>
```

2.路径通配符匹配

在书写url-pattern的时候，必须以/开始，后面可以使用*号表示任意的匹配

配置：

```xml
<url-pattern>/DemoServlet/*</url-pattern>
```

外界在访问的时候，只要能够和/DemoServlet匹配上，后面写任何东西都可以

http://localhost:8080/test/DemoServlet/11111/aaa

3.扩展名匹配在书写url-pattern的时候，不能以/开始，以*开始，后面写扩展名

配置：

```xml
<url-pattern>*.do</url-pattern>
```

常见的扩展名：

​    *.action  *.do  *.go

访问的方式：

http://localhost:8080/test/xxxx.do

url-pattern标签中的路径可以按照上述的三种书写，它们的优先级：

​        全路径匹配  >  路径通配符匹配  >  扩展名匹配

#### 



#### 

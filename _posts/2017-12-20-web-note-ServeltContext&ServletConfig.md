---
layout: post
title:  "ServeltContext&ServletConfig"
date:   2017-12-20
categories: web
tags: ServeltContext ServletConfig
---

* content
{:toc}
## ServletConfig

每一个servlet都跟着一个独立的ServletConfig对象。

ServletConfig包含了一些Servlet的配置信息。在调用servlet的init方法时,将servletCongfig传给servlet。

<!-- more -->

servletConfig的内容是  是自己定义的. 

在web.xml中 的servlet标签下书写servletConfig的内容.

```xml
  <servlet>
    <servlet-name>DemoServlet</servlet-name>
    <servlet-class>cn.itcast.web.DemoServlet</servlet-class>
    <!-- 给指定的Servlet配置初始化参数信息 ，参数是key、value组成-->
    <init-param>
    	<param-name>Class</param-name>
    	<param-value>一年级</param-value>
    </init-param>
  </servlet>
  <servlet-mapping>
    <servlet-name>DemoServlet</servlet-name>
    <url-pattern>/demo</url-pattern>
  </servlet-mapping>v
```

## **ServletContext（全局容器）**：

有些数据我们希望多个Servlet共享、同时使用，ServletContext容器保存的数据就是全局数据，被所有Servlet共享。

当tomcat启动时，会为每个web应用创建一个唯一的ServletContext对象代表当前Web应用.该对象不仅封装了当前web应用的所有信息，而且实现了多个servlet的数据共享.

在每个项目中可以有多个Servlet程序，每个Servlet程序都是独立的。Servlet中的配置信息可以使用ServletConfig获取，而当前这个项目的配置信息，就必须使用描述这个项目的ServletContext对象获取。

tomcat会读取servlet中init-param这个标签，把里面的内容封装到ServletConfig中，把封装好的 servletConfig对象, 传递给servlet中.调用servelt中的init(ServletConfig config)

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171220/Iblj2IcljJ.jpg?imageslim)

```java
public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		ServletConfig config = getServletConfig();
		String cla = config.getInitParameter("Class");
		String servletName = config.getServletName();
		ServletContext context = config.getServletContext();
		Enumeration<String> names = config.getInitParameterNames();
		while(names.hasMoreElements()){
			System.out.println(names.nextElement());
		}
	}

```

ServletContext容器的存取删操作：

```java
public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //获取ServletContext对象
		ServletContext servletContext = getServletContext();
        //设置数据
		servletContext.setAttribute("hehe", "呵呵");
        //获取数据
		String hehe = (String) servletContext.getAttribute("hehe");
		System.out.println("hehe:"+hehe);
        //移除数据
		servletContext.removeAttribute("hehe");
		String hehe2 = (String) servletContext.getAttribute("hehe");
		System.out.println("hehe:"+hehe2);
	}
```

ServletContext 获取全局配置数据和ServeltConfig类似

```xml
	<context-param>
		<param-name>company</param-name>
		<param-value>百度</param-value>
	</context-param>
	<context-param>
		<param-name>address</param-name>
		<param-value>北京中关村</param-value>
	</context-param>
```





#### 

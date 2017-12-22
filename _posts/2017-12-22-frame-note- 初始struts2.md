---
layout: post
title:  "Struts2"
date:   2017-12-22
categories: frame
tags: Struts2
---

* content
{:toc}
### 什么是Struts2：

Struts2是一个基于MVC设计模式的WEB层框架。

### MVC设计模式：

MVC,全名是Model View Controller，是模型(Model)－视图(View)－控制器(Controller)的缩写，是一种软件设计模式，或软件设计思想。

<!-- more -->

#### MVC的具体含义：

1. Model:数据模型，用来处理数据，一般是一个实体类，例如User类；
2. View:视图，用来显示界面，可以是JSP或Html；
3. Controller:控制器，用来决定哪个界面来展示数据模型；

MVC设计模式有多种实现方式，其中，JSP+Servlet+JavaBean就是其中的实现方式之一。但Servlet存在着接收请求参数、页面跳转代码冗余的问题。struts2则可以解决这些问题。

注意：MVC和JavaEE三层架构规范不是同一个概念，一定要区分开来。

### Struts2环境搭建

第一步导入jar包：

​    官网地址：<http://struts.apache.org/>

第二步在web.xml中配置Struts2的核心过滤器:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd" id="WebApp_ID" version="3.0">
  <display-name>struts2_day01</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>
  	<!-- 
  		配置Struts2框架的核心过滤器
  	 -->
   <filter>
        <filter-name>struts2</filter-name>    
        <filter-class>
org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter
        </filter-class>
   </filter>
    <filter-mapping>
        <filter-name>struts2</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
</web-app>
```

第三步在类的根路径下创建一个名叫struts.xml的文件，并导入约束

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE struts PUBLIC
	"-//Apache Software Foundation//DTD Struts Configuration 2.3//EN"
	"http://struts.apache.org/dtds/struts-2.3.dtd">
<struts>
    
	<package name="default" namespace="/" extends="struts-default">
	    
	</package>
    
</struts>
```

### Struts_入门（请求与跳转）：

1. 编写Action类，负责接收用户请求，并处理请求

```java
public class HelloAction extends ActionSupport{

	
	private static final long serialVersionUID = 1L;

	/**
	 * 重写父类的execute方法，请求进来，默认执行execute方法
	 */
	@Override
	public String execute() throws Exception {
		//调用业务的代码
		System.out.println("Hello World");
		//框架获取到该方法的返回值，返回值对应的要跳转到页面
		return "success";
	}

}

```

2. 在struts.xml中配置HelloAction类

```java
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE struts PUBLIC
	"-//Apache Software Foundation//DTD Struts Configuration 2.3//EN"
	"http://struts.apache.org/dtds/struts-2.3.dtd">
<struts>
	<package name="default" namespace="/" extends="struts-default">
		<!-- 
			name:Action类的名字，具有唯一性，页面上通过name来访问Action类
		 -->
		<action name="hello" class="cn.itcast.action.HelloAction">
			<!-- 配置结果集 :name属性的值一定要与execute方法的返回值一致-->
			<result name="success">
				/success.jsp
			</result>
		</action>
	</package>
</struts>
```

3. 在WebContent下新建success.jsp页面

4. 启动Tomcat测试访问结果

   启动tomcat,在浏览器中输入地址：http://localhost:8080/struts2_day01/hello.action或http://localhost:8080/struts2_day01/hello

注意：Struts2框架规定action默认的后缀名就是action，也可以不写，一般都带上action。

### Struts2配置文件详解

#### 框架内置的配置文件: 

1. default.properties:在struts2-core.jar的org.apache.struts2包中，存放了Struts2所有的常量信息
2. struts-default.xml:在struts2-core.jar中，定义了Struts2框架底层的一些bean,struts-default抽象包，拦截器、结果集
3. struts-plugin.xml:可有可无，在某个插件的jar包中，定义了插件中的bean、action等信息

#### 自定义的配置文件:

1. struts.xml:是strust2的核心配置文件，位于工程的src目录下,主要用于配置用户自定义的Action类
2. struts.properties:可有可无，位于工程的src目录下，用户可以添加，也可以不添加，用于配置Sturts2的常量
3. web.xml：在WEB-INF下，也可以配置Struts2的常量

#### 配置文件加载的顺序:

Struts2框架中的六个配置加载顺序如下：

​	default.properties--->struts-default.xml--->struts-plugin.xml--->struts.xml--->struts.properties--->web.xml

#### `核心配置文件struts.xml`

##### constant标签:用于修改struts2中的常量

```xml
<!-- 开启开发者模式 -->
<constant name="struts.devMode" value="true"></constant>
```

开启开发者模式的好处：

​	当发生异常后，可以在页面上看到详细的异常信息

##### package标签:

name：

​        包的名称。必须写。且必须唯一。

​    extends：

​        一般情况下需要继承struts-default包，但不是必须的。不过如果不继承的话，将无法使用struts2提供的核心功能。struts-default.xml中定义着struts-default这个包。而struts-default.xml是在我们的struts.xml加载之前加载。

​    abstract：

​        把包声明为抽象包，抽象包就是用来被继承的。只要是没有&lt;action&gt;元素的包，就可以声明为抽象包。

​    namespace：

​        名称空间。它的作用是把访问的URL按照模块化来管理。

​        名称空间的写法：

​              一般以/开头

​              当我们指定了名称空间之后，访问的URL就变成了：

​                                名称空间+action标签的name属性取值

​              例如：

​                /n1/hello.action

##### `action标签:`

​    配置Action类

​    	name：指定的Action类的访问名称。注意此处不能有后缀。必须唯一。

​    	class：指定的是Action类的全限定类名。

​    	method：指定的是Action中方法名称





### Action的编写:

1. 实现Action接口

   好处：可以利用Action接口里的静态常量作为方法的返回值


2. 继承ActionSupport父类，重写execute方法

   ActionSupport已经实现了Action接口,开发常用。

   通过设置method属性调用Action中的方法

   ```xml
   <action name="add" class="action.DeptAction" method="add"></action>
   <action name="list" class="action.DeptAction" method="list"></action>
   <action name="update" class="action.DeptAction" method="update"></action>
   ```

### 通配符的使用：

在struts2中用*表示通配符：匹配所有路径，{1}取出第一个*的值，{2}取出第二个*的值

上面的action可以简写成：

```xml
<action name="*" class="action.DeptAction" method="{1}"></action>
```

有两个通配符的情况

```xml
<action name="*_*" class="action.{1}Action" method="{2}"></action>

<action name="Customer_list" class=".action.CustomerAction" method="list"></action>
```


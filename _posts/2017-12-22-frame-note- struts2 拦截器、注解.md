---
layout: post
title:  "Struts2拦截器、注解"
date:   2017-12-22
categories: frame
tags: 拦截器 
---

* content
{:toc}
## Struts2的工作原理

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171222/5kfk4icidc.png?imageslim)

<!-- more -->

### 在Struts2框架中的处理大概分为以下几个步骤 

1 客户端初始化一个指向Servlet容器（例如Tomcat）的请求 

2 这个请求经过一系列的过滤器（Filter）（这些过滤器中有一个叫做ActionContextCleanUp的可选过滤器，这个过滤器对于Struts2和其他框架的集成很有帮助，例如：SiteMesh Plugin） 

3 接着FilterDispatcher被调用，FilterDispatcher询问ActionMapper来决定这个请是否需要调用某个Action 

4 如果ActionMapper决定需要调用某个Action，FilterDispatcher把请求的处理交给ActionProxy 

5 ActionProxy通过Configuration Manager询问框架的配置文件，找到需要调用的Action类 

6 ActionProxy创建一个ActionInvocation的实例。 

7 ActionInvocation实例使用命名模式来调用，在调用Action的过程前后，涉及到相关拦截器（Intercepter）的调用。 

8 一旦Action执行完毕，ActionInvocation负责根据struts.xml中的配置找到对应的返回结果。返回结果通常是（但不总是，也可 能是另外的一个Action链）一个需要被表示的JSP或者FreeMarker的模版。在表示的过程中可以使用Struts2 框架中继承的标签。在这个过程中需要涉及到ActionMapper

参考：http://blog.csdn.net/laner0515/article/details/27692673/

## 拦截器

当请求struts2的action时，Struts 2会查找配置文件，并根据其配置实例化相对的    拦截器对象，然后串成一个列表，最后一个一个地调用列表中的拦截器.简而言之就是用来拦截器请求的

\1. Struts2拦截器是在访问某个Action或Action的某个方法，字段之前或之后实施拦截，并且Struts2拦截器是可插拔的，拦截器是ＡＯＰ的一种实现．

\2. 拦截器栈（Interceptor Stack）。Struts2拦截器栈就是将拦截器按一定的顺序联结成一条链。在访问被拦截的方法或字段时，Struts2拦截器链中的拦截器就会按其之前定义的顺序被调用。

过滤器和拦截器的区别：

1. 过滤器可以过滤所有的请求（页面，Servlet）,拦截器只能拦截Action请求，不能拦截器页面。
2. 拦截器不需要依赖Servlet容器，过滤器需要依赖Servlet容器（需要有servlet的api）

### 自定义拦截器

1. 实现Interceptor接口，重写init、interceptor、destory方法

```java
public class Test1Interceptor implements Interceptor {
	
	private static final long serialVersionUID = 1L;

	@Override
	public String intercept(ActionInvocation invocation) throws Exception {
		System.out.println("请求经过了第一个测试拦截器");
		String result = invocation.invoke();
		System.out.println("响应经过一号拦截器");
		return result;
	}
	

	@Override
	public void destroy() {

	}

	@Override
	public void init() {

	}

}
```

2. 继承AbstractInterceptor父类，重写interceptor方法

```java
public class Test2Interceptor extends AbstractInterceptor {
	@Override
	public String intercept(ActionInvocation invocation) throws Exception {
		System.out.println("请求经过了第二个拦截器");
		String result= invocation.invoke();
		System.out.println("响应经过二号拦截器");
		return result;
	}

}
```

3. 继承MethodFilterInterceptor，重写doInterceptor方法

```java
public class Test3Interceptor extends MethodFilterInterceptor {

	@Override
	protected String doIntercept(ActionInvocation invocation) throws Exception {
		System.out.println("请求经过了第三个拦截器");
		String result = invocation.invoke();
		System.out.println("响应经过三号拦截器");
		return result;
	}

}
```

第三种方式较于第二种方式的是通过第三种方式可以指定拦截某些方法，不拦截某些方法，比较灵活。

#### 配置自定义拦截器

局部拦截器配置：

新建action类，

配置文件：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE struts PUBLIC
	"-//Apache Software Foundation//DTD Struts Configuration 2.3//EN"
	"http://struts.apache.org/dtds/struts-2.3.dtd">
<struts>
    <constant name="struts.enable.DynamicMethodInvocation" value="false" />
    <constant name="struts.devMode" value="true" />
    <package name="default" namespace="/" extends="struts-default">
    	<interceptors>
    		<!-- 声明自定义的拦截器类 -->
    		<interceptor name="test1Interceptor" class="cn.itcast.interceptor.Test1Interceptor"></interceptor>
    		<interceptor name="test2Interceptor" class="cn.itcast.interceptor.Test2Interceptor"></interceptor>
    		<interceptor name="test3Interceptor" class="cn.itcast.interceptor.Test3Interceptor"></interceptor>
    	</interceptors>
    	<action name="test1" class="cn.itcast.action.Test1Action">
    		<interceptor-ref name="test1Interceptor"></interceptor-ref>
    		<interceptor-ref name="test2Interceptor"></interceptor-ref>
    		<interceptor-ref name="test3Interceptor"></interceptor-ref>
    		<!-- 一定要引入默认的拦截器栈，因为一旦引入自定义的，默认的会失效 -->
    		<interceptor-ref name="defaultStack"></interceptor-ref>
    		<result>
    			/test1.jsp
    		</result>
    	</action>
    </package>
</struts>
```

```xml
<interceptors></interceptors>标签用于声明标签类
<interceptor-ref ></interceptor-ref>指定test1经过哪些标签
```



## Struts注解

#### @ParentPackage

用于指定当前Action类所在包的父包。

#### @Namespace

指定当前Action中所有动作方法的名称空间。

#### @Action

指定当前方法的访问路径。

​    value：指定访问路径。

​    results[]：它是一个数组，数据类型是注解。用于指定结果视图。此属性可以没有，当没有该属性时，表示不返回任何结果视图。即使用response输出响应正文。

​    interceptorRefs[]：它是一个数组，数据类型是注解。用于指定引用的拦截器。

#### @Result

出现在类上，表示当前Action类中的所有动作方法都可以用此视图。

#### @Results

用于配置多个结果视图。

#### @InterceptorRef

用于配置要引用的拦截器或者拦截器栈

1. 出现在Action的方法中：表示只有该请求经过该拦截器，其它的请求还是经过web.xml中配置的默认栈
2. 出现在Action类上：表示该类中所有的请求都经过该拦截器

```java
@ParentPackage("default")
@Namespace("/xxx/ooo")
@Results({ @Result(name = "fail", location = "/fail.jsp") })
public class Test1Action extends ActionSupport {

	private static final long serialVersionUID = 1L;

	@Action(value = "test1", results = {
			@Result(name = "success", type = "redirect", location = "/test1.jsp") }, interceptorRefs = {@InterceptorRef("myStack") })
	@Override
	public String execute() throws Exception {
		System.out.println("请求进入了execute方法...");
		return SUCCESS;
	}

	@Action("test2")
	public String execute1() throws Exception {
		System.out.println("请求进入了execute1方法...");
		return "fail";
	}

}
```

```java
@ParentPackage("default")
@Namespace("/xxx/ooo")
@Results({ @Result(name = "fail", location = "/fail.jsp") })
@InterceptorRef("myStack")
public class Test1Action extends ActionSupport {
}
```


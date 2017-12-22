---
layout: post
title:  "Struts中的servlet"
date:   2017-12-12
categories: frame
tags: Struts中的servlet
---

* content
{:toc}
## Action使用Servlet相关API

Action的本质就是一个Servlet。Action是对Servlet的封装，实现了与Servlet相关的API的解耦。

### 一、ActionContext类

<!-- more -->

ActionContext类的对象表示Action的上下文对象

1. ActionContext.getContext().get(“request”)：返回值是Map，需要强转，相当于是一个解耦之后的HttpServletRequest
2. ActionContext.getContext().getSession()：返回值是Map，相当于解耦之后的HttpSession
3. ActionContext.getContext().getApplication()：返回值是Map，相当于解耦之后的ServletContext
4. ActionContext.getContext().getParamerter()：返回值是Map，存储的是所有参数信息，键是参数名，值是具体的参数值，类型是数组类型

```xml
Map<String,Object> req  = (Map<String, Object>) Map<String,Object> req  = (Map<String, Object>) 
req.put("username", "tom");
```

测试发现页面中的el表达式    ${requestScope.username}     能取出tom ,说明其底层也是HttpServletRequest

```xml
    //HttpSession
		Map<String,Object> session = ActionContext.getContext().getSession();
    //ServletContext
		Map<String,Object> application = ActionContext.getContext().getApplication();
    //传入的参数
    Map<String,Object> params = ActionContext.getContext().getParameters();
```

### 二、Aware接口

Sruts2框架提供了一些Aware接口，帮助我们访问Servlet API:

- Action类实现RequestAware接口，框架会把HttpServletRequest对象解耦成Map，注入到Action类中
- Action类实现SessionAware接口，框架会把HttpSession对象解耦成Map，注入到Action类中
- Action类实现ApplicationAware接口，框架会把ServletContext对象解耦成Map，注入到Action类中
- Action类实现ParameterAware接口，框架会把请求中所有的参数信息封装成Map，注入到Action类中

```xml
public class Test2Action extends ActionSupport implements RequestAware,SessionAware,ApplicationAware,ParameterAware{
private Map<String,Object> request;
	private Map<String, Object> session;
	private Map<String, Object> application;
	private Map<String, String[]> parameters;
	
	@Override
	public String execute() throws Exception {
		request.put("username", "张三");
		System.out.println(request.get("username"));
		System.out.println(parameters.get("username")[0]);
        
		return SUCCESS;
	}
    	/**
	 * strusts2会自动调用setRequest方法，把HttpServletRequest解耦后得到的那个map通过该方法注入到程序里来
	 */
	@Override
	public void setRequest(Map<String, Object> request) {
		System.out.println("调用了setRequest方法");
		this.request = request;
	}

	@Override
	public void setSession(Map<String, Object> session) {
		this.session = session;
	}


	@Override
	public void setApplication(Map<String, Object> application) {
		this.application = application;
	}

	@Override
	public void setParameters(Map<String, String[]> parameters) {
		this.parameters = parameters;
	}	

}
```

### 三、ServletActionContext类

- ServletActionContext.getRequest()：获取HttpServletRequest对象
- ServletActionContext.getResponse()：获取HttpServletResponse对象
- ServletActionContext.getRequest().getSession()：获取HttpSession对象
- ServletActionContext.getServletContext()：获取ServletContext对象

```xml
public class Test3Action extends ActionSupport{

	private static final long serialVersionUID = 1L;
	
	@Override
	public String execute() throws Exception {
		HttpServletRequest req = ServletActionContext.getRequest();
		ServletActionContext.getResponse();
		HttpSession session = ServletActionContext.getRequest().getSession();
		ServletContext sc = ServletActionContext.getServletContext();
		return NONE;
	}

}
```

ActionContext  +  ***Aware  这两种方式成为  “间接调用Servlet api”（解耦）（推荐）

ServletActionContext的方式称为 “直接方法”（非解耦）
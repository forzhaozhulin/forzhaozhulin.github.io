---
layout: post
title:  "OGNL表达式和值栈"
date:   2017-12-22
categories: frame
tags: OGNL表达式
---

* content
{:toc}


## OGNL表达式

OGNL表达式主要用于页面的取值，类似于EL表达式。（OGNL表达式需要结合s标签来使用）

OGNL的四种简单用法：

1. 直接显示一个值
2. 调用对象的方法或属性
3. 可以直接调用类里的静态方法，把调用静态方法的开关打开
4. 计算某个表达式的值

<!-- more -->

注意：在演示ognl调用静态方法时，需要把静态方法调用的开关打开

struts.xml中：

```xml
<!-- 打开通过OGNL表达式调用静态方法的开关 -->
<constant name="struts.ognl.allowStaticMethodAccess" value="true"></constant>
```

```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib uri="/struts-tags" prefix="s"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>演示ognl的几种用法</h1>
	直接在页面上显示一个值：<s:property value="1"/><br>
	调用对象里的方式：<s:property value="'hello'.toUpperCase()"/><br>
	直接调用类里静态方法：<s:property value="@java.lang.Math@max(10,11)"/><br>
	算术运算：<s:property value="1+2"/>
</body>
</html>

```



## 值栈：

值栈是struts2框架的数据中转站，里面保存了action对象、web对象、自定义保存的对象，随用随取。

每个请求都会产生一个值栈，会放在request域中，名字struts.valueStack

获取值栈的两种方式：

1. 从request域中获取
2. 通过ActionContext中的getValueStack方法获取

```java
public class ValueStackAction extends ActionSupport{

	private static final long serialVersionUID = 1L;
	
	@Override
	public String execute() throws Exception {
		Map<String,Object> req = (Map<String, Object>) ActionContext.getContext().get("request");
		ValueStack valueStack = (ValueStack) req.get("struts.valueStack");
		
		ValueStack valueStack1 = ActionContext.getContext().getValueStack();
		System.out.println(valueStack == valueStack1);
		return SUCCESS;
	}

}
```

值栈包含两个逻辑部分：对象栈+上下文栈

​        对象栈（List）存放：Action对象+其它对象

​        上下文栈（Map）存放：Action对象，web对象的引用，其它对象

### 向值栈的手动存放数据：

```java
public class ValueStack1Action extends ActionSupport{

	private static final long serialVersionUID = 1L;
	
	@Override
	public String execute() throws Exception {
		ValueStack valueStack = ActionContext.getContext().getValueStack();
		//向对象栈里存放数据（匿名存放）
		valueStack.push("汪峰");
		//向对象栈里存放数据（有名字的存放），实质是存放了一个Map进去
		valueStack.set("username", "那英");
		
		//向上下文栈里方数据
		ActionContext.getContext().put("username", "哈林");
		return SUCCESS;
	}

}
```

###  取值栈里的数据

值栈的中的数据需要通过OGNL表达式结合s标签来获取

```xml
	取对象栈里的数据：<s:property value="username"/><br>
	取对象栈里匿名的数据：<s:property value="[1].top"/><br>
	取对象栈栈顶的元素：<s:property value="[0].top.username"/><br>
    取上下文栈里的数据：<s:property value="#username"/><br>
```

取上下文栈的数据要加#，而取对象栈的数据只要根据属性名就行了。

### 值栈的搜索顺序

1. 如果不加#号，先搜索对象栈，再搜索上下文栈，如果都都搜索不到，就不显示；如果中途搜索到，立即停止搜索；
2. 如果加#号，直接搜索上下文栈，如果搜索不到，就不显示；

### 值栈的生命周期

1. 每一个请求都会产生一个值栈对象，放在request的域中，名字叫struts.valueStack
2. 值栈的生命周期同request的生命周期，如果请求的是action，值栈的生命周期同action的生命周期
3. 值栈贯穿整个Action的生命周期
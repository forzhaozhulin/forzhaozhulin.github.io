---
layout: post
title:  "Servlet学习笔记---Cookies"
date:   2016-03-29 09:56:20
categories: JavaWeb
tags: Servlet
author: 薛彬
---

* content
{:toc}






今天，我学习了如果制作cookie，对，甜点。

![](http://i.imgur.com/BigLwhy.png)

按照往常一样，我需要记录一下笔记，我们是要做一个奶油曲奇呢还是巧克力曲奇呢？

咳咳，言归正传，我说的cookie可不是吃的。

## 什么是cookie

我们来看看维基百科中关于cookie的解释：

>中文名称为“小型文本文件”，指某些网站为了辨别用户身份而储存在用户本地终端（Client Side）上的数据（通常经过加密）。

也就是说cookie是一小段的文本信息，Web服务器将它发送到浏览器，游览器会将它保存在本地。之后，在访问同一网站时，浏览器又将它返回给服务器。比如通过让服务器验证cookie，可以实现访问者的免密再次登陆。

客户端和服务器的连接是基于一种请求应答的模式，也就是说服务器和客户端会建立一个连接，在客户端发送请求后并且服务端做出响应后，这个连接就会断开了。

如果按照这个逻辑，在有登陆需求的网站中，一个用户在其中一个页面登陆后跳转到了这个网站的其他子页面中，这时候用户的状态还是登陆的吗？你有没有遇到过让你重复登陆的情况呢？

这时候就需要让网站“记住”用户，当这个用户再次访问的时候，网站可以识别出这个用户让他不用登陆就可以访问，cookie就派上用场了。

总得来说，cookie识别返回用户有三个步骤：

- 服务器向浏览器发送一组cookies。
- 浏览器将这些cookies存储在用户本地，以备后来使用。
- 当下次用户登陆同个网站时，浏览器会向服务器发送请求，这时浏览器会先把cookie发送到服务器，服务器就可以根据cookie来判定这个用户是否登陆过。

## cookie的使用

知道了什么是cookie之后，我们就要使用它。

### 创建cookies

```java
//创建一个cookie对象
Cookie cookie =new Cookie("key","value");
//设置它的最大生存周期为一周（以秒为单位）
cookie.setMaxAge(60*60*24*7);
//发送cookie到HTTP响应头
response.addCookie(cookie);
```

### cookies的相关方法

下表列出了Servlet中操作cookies时常用的方法：

|方法名|描述|
|---|:---|
|setDomain(String)|设置cookie适用的域。|
|getDomain()|得到cookie适用的域。|
|setMaxAge(int)|设置cookie过期的时间（单位：秒）。|
|getMaxAge()|得到cookie过期的时间（单位：秒）。|
|getName()|获得cookie的名称。|
|setValue(String)|设置cookie关联的值。|
|getValue()|得到cookie关联的值。|

### 读取cookies

传统的cookies的读取分为两个步骤：

- 调用request.getCookies得到一个cookies对象的数组。
- 对数组进行遍历，调用每个cookie的getName方法和个体Value方法。

我们可以这样读取cookies：

```java
Cookie[] cookie=request.getCookies();
if(cookies!=null){
	for(int i=0;i<cookies.length;i++){
		Cookies cookie =cookies[i];
		String name=cookie.getName();
		String value=cookie.getValue();
	}
}
```

这样就将cookie的name和value得到了。

### 实用程序

#### getCookieValue方法

getValue方法是需要遍历所有cookie才能得到值，如果我们想得到某一个name对应的value呢？getCookieValue()这个方法就可以满足这个需求。getCookieValue()这个方法是在CookieUtilities这个类中，我们看看getCookieValue()的源码。

```java
public static String getCookieValue(HttpServletRequest request,String cookieName,String defaultValue){
	Cookie[] cookies=request.getCookies();
	if(cookies!=null){
	for(int i=0;i<cookies.length;i++){
		Cookies cookie =cookies[i];
		if(cookieName.equals(cookie.getName())){
			return(cookie.getValue());
		}
	}
	return(defaultValue);
}
```

#### LongLivedCookie类

这个类是可以自动将cookie保持一年。

我们也来看看源码：

```java
public class LongLivedCookie extends Cookie{
	public static final int SECONDS_PER_YEAR=60*60*24*365;
	public LongLivesCookie(String name,String value){
		super(name,value);
		setMaxAge(SECONDS_PER_YEAR);
	}
}
```

我们就可以直接用这个类来创建cookie对象：

```java
LongLivedCookie newCookie=new LongLivedCookie("name","value");
```

## 总结

cookie是掌握web开发必须会的知识点，这次的笔记只是入门，还需深入学习。
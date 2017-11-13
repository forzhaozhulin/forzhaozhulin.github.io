---
layout: post
title:  "Servlet学习笔记---Session"
date:   2016-03-30 13:44:20
categories: JavaWeb
tags: Servlet
author: 薛彬
---

* content
{:toc}





今天学习了session的有关知识。对于我而言，cookies之前还在清除浏览器记录的时候见过，而session则是一个完全陌生的单词，第一次在计算机中见到。

## 什么是session

session的中文翻译是“会话”。在Web开发中，session代表的是浏览器与服务器连接的一个过程。


## 为什么要用session

这个问题我们首先设定一个场景：你正在超市货架上选购商品，将购物车放在了走道上，此时超市工作人员看到了落单的购物车，他是否会将你购物车中的商品拿出摆上货架呢？ 这时候工作人员需要确定这些商品是你选择的。再看一种情况，你选好商品之后，发现自己的钱包落在了车上，需要出超市去取钱包，这时候工作人员看到落单的购物车是否会把其中的商品拿出来？

将这个场景类比到Web网站中，你打开了一个Web站点，浏览并登陆了你的账号，此时你如果打开这个站点的子链接，或是不小心离开了这个网站。过了一会儿，你又回到了这个网站，此时如果再让你登陆一次你是不是会感到厌烦？

首先，我们了解一下HTTP，Web所使用的HTTP协议是无状态的，每次请求之间都是独立的，服务器自动不保留之前客户的请求的任何记录。

这时候就需要用到session，就像是在超市中，你会和工作人员说我去取一下钱包或是工作人员会观察购物车一段时间，若没人来取就会拿出商品。**session就是这样一个在Web中用来在客户端与服务器端之前保持状态的解决方法**。

### 三种典型解决方案

对于上面出现的情况，存在着3种典型的解决方案。

- cookies：一个Web服务器可以分配一个唯一的session会话ID作为每个Web客户端的Cookies。
- 隐藏的表单字段：Web服务器可以发送一段隐藏的HTML表单字段。`<Input type="hidden" name="session" value="a123">`。在提交表单时，要将制定的名称和值自动包括在GET或POST数据中。
- URL重写：客户程序在每个URL的尾部添加一些额外数据。这些数据标识当前的会话，服务器会把该会话标识与已存储的有关会话的数据项关联。


## session的使用

session的使用我们需要用到HttpSession，这是servlet提供的一个接口。这个借口提供了一种跨多个页面请求或访问网址时识别用户以及存储用户信息的方式。

servlet中就是用这个号借口创建一个客户端和服务器之间的session会话，持续一个用户定义的时间段。

会话的使用可以分为四个步骤：

- 访问与当前请求相关联的会话对象；
- 查找与会话相关联的信息。
- 存储会话中的信息。
- 废弃会话数据。

### HttpSession对象

通过调用HttpServletRequest的getSession方法来访问HttpSession对象。

我们可以这样创建一个session：

```java
HttpSession session =request.getSession();
```

### session的相关方法

下表列出了Servlet中操作cookies时常用的方法：

|方法名|描述|
|---|:---|
|getAttribute(String)|返回在该 session 会话中具有指定名称的对象。|
|setAttribute(String, Object) |使用指定的名称绑定一个对象到该 session 会话。|
|getCreationTime()|该方法返回该 session 会话被创建的时间。|
|getId()|返回一个包含分配给该 session 会话的唯一标识符的字符串。|
|getLastAccessedTime()|返回客户端最后一次发送与该 session 会话相关的请求的时间。|
|getMaxInactiveInterval()|返回 Servlet 容器在客户端访问时保持 session 会话打开的最大时间间隔（单位：秒）。|
|setMaxInactiveInterval(int)|设置该 session 会话无效之前，指定客户端请求之间的时间。|


## 总结

算是吧session的入门知识给搞明白了，还需通过敲代码来深入理解。


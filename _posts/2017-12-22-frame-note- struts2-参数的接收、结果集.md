---
layout: post
title:  "struts2-参数的接收、结果集"
date:   2017-12-22
categories: frame
tags: 模型驱动 属性驱动 结果集
---

* content
{:toc}
struts2 最初就是为了解决请求参数、和跳转页面代码冗余的问题。

请求参数接收的方式有三种，分为两大类：属性驱动和模型驱动。

## 属性驱动：

简单属性驱动方式接收：页面提交用户名和密码到后台。

<!-- more -->

在Action类中定义属性，提供属性对应的setter方法

注意：属性的名字要与参数名一样

```xml
public class Login1Action extends ActionSupport{
	private String username;//属性名一定要和页面参数名一致
	private String password;//属性名一定要和页面参数名一致
	@Override
	public String execute() throws Exception {
		System.out.println(username);
		System.out.println(password);
		return SUCCESS;
	}

	public void setUsername(String username) {
	}

	public void setPassword(String password) {
		this.password = password;
	}
}
```

### 对象属性驱动方式接收：

页面表单:

```html
	<form action="login2.action" method="post">
		用户名：<input type="text" name="user.username"><br>
		密码：<input type="password" name="user.password"><br>
		<input type="submit" value="登录">	
	</form>
```

实体类User：

```java
public class User {
	private String username;//属性名要和参数名一致，并且要提供对应的set方法
	private String password;
    //提供相对应的get set 方法
}
```

Action类

```java
public class Login2Action extends ActionSupport{	
	private User user;//对象属性接收参数,名字要和页面上对象名一致
	@Override
	public String execute() throws Exception {
		System.out.println(user.getUsername());
		System.out.println(user.getPassword());
		return SUCCESS;
	}
	public User getUser() {
		return user;
	}
	public void setUser(User user) {
		this.user = user;
	}
}
```

## 模型驱动

1. 页面是正常的username和password
2. 实体类封装同对象属性
3. 在Action类中提供对象属性并实例化，让Action类实现ModelDriven<实体类>接口，重写getModel方法，返回对象属性。

```java
public class Login3Action extends ActionSupport implements ModelDriven<User>{	
	private User user = new User();//一定要手动实例化
	@Override
	public String execute() throws Exception {
		System.out.println(user.getUsername());
		System.out.println(user.getPassword());
		return NONE;
	}
	/**
	 * struts2先判断Action类是否实现了ModelDriven
	 * 如果实现，调用getModel方法，返回一个对象，框架会把参数填充到返回的对象里
	 */
	@Override
	public User getModel() {
		return user;
	}
}
```

如果参数比较少（一到两个），可以使用第一种；

如果参数比较多，建议采用第三种

### 当简单属性和模型对象共存时，

模型对象里的属性有值，简单属性没有值。

原因：与值栈相关，模型对象在栈顶，在当前Action的实例前，注入参数时，优先找到的是模型对象。



## result结果集

###  result标签：

​    作用：配置结果集，指定跳转到哪个页面

​    name属性:结果集的名字，与Action的方法的返回值对应，一个Action节点可以对应多个结果集，默认值是success，可以省略

### 全局结果集和局部结果集

全局结果集：针对某个包下所有的action都有效

局部结果集：只针对某个Action有效

```java
<global-results>
    <result name="error">
        /error.jsp
    </result>
</global-results>
```

### 结果集类型

- dispatcher：转发到一个页面，地址栏不会发生变化。result节点type属性的默认是就是dispatcher。
- redirect：重定向到一个页面，地址栏会发生变化。
- chain：转发到另一个action，地址栏不会发生变化。
- redirectAction：重定向到另一个action，地址栏会发生变化。
- stream：以流的形式响应一个请求，文件下载时会用到。

```xml
<result type="dispatcher">
      /hello.jsp
</result>
```


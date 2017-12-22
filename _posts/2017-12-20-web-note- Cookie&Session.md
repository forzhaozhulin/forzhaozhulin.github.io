---
layout: post
title:  "Cookie&Session"
date:   2017-12-20
categories: web
tags: Cookie Session
---

* content
{:toc}
## Cookie

Cookie:是在客户端保存数据的技术，

例如：我们登陆一些网站时在填写账户密码后选择记住账号

然后下一次登陆时就会自动出现上次登陆的账号，

账号的数据保存在浏览器的Cookie中。

<!-- more -->

### Cookie的快速使用

建立Cookie对象发送数据给浏览器

```java
		//调用构造函数，创建Cookie对象
		Cookie cookie = new Cookie("username", "tom");
		//通过响应对象将cookie发送给浏览器
		response.addCookie(cookie);
```

从浏览器获取Cookie数据

```java
		//获取Cookie数组
		Cookie[] cookies = request.getCookies();
		for (Cookie cookie : cookies) {
			//cookie.getName() 获取cookie的名称
			String name = cookie.getName();
			//取出cookie的值
			String value = cookie.getValue();
			System.out.println(name+":"+value);
		}
```

### Cookie的生命周期：

在cookie创建的时候设置cookie的生存时间，时间一到自动去死

如果不设置过期时间，默认是浏览器关闭的时候。

```java
		//调用构造函数，创建Cookie对象
		Cookie cookie = new Cookie("username", "tom");
		//设置生存时间，单位是秒
		cookie.setMaxAge(30);
		//通过响应对象将cookie发送给浏览器
		response.addCookie(cookie);
```

路径问题：

cookie可以通过setPath(String url)来设置路径。

```java
		//调用构造函数，创建Cookie对象
		Cookie cookie = new Cookie("username", "tom");
		//设置生存时间，单位是秒
		cookie.setMaxAge(30);
		//设置Cookie的访问路径
		cookie.setPath("/cookie/demo/");
		//通过响应对象将cookie发送给浏览器
		response.addCookie(cookie);
```

### 删除Cookie:

删除cookie其实是发送一个新的cookie，设置生成时间为0,而且设置数据为空字符或则null，通过response对象发送之后，会]覆盖之前的cookie。

注意，删除cookie时，path必须一致，否则不会删除

1. 将cookie的name（key）保持一致，value 设置为 ""; 

   ​    cookie = new Cookie("username","")

2. 设置存活时间为0，

   ​    cookie.setMaxAge(0)

3. 路径要发送cookie时保持一致，没有路径不需要设置。

   ​    cookie.setPath("/");

4. 将cookie发送给浏览器。

   ​    response.addCookie(cookie)

## Session：服务器端的会话技术

 Cookie技术可以将用户的信息保存在各自的浏览器中。

优点：很明显实现了同一个用户不同请求中数据共享。

缺点：黑客可以利用脚本等手段 窃取cookie中的重要数据，从而泄漏个人的隐私，存在巨大的安全隐患。

Cookie数据保存在用户电脑中，不是每个用户都是电脑高手，对数据安全措施，做的必然不完善。

诞生新技术：session，在服务器端保存用户的数据。（注意：session技术，还是依赖cookie技术）

1. session是服务器开辟的一个用来存储数据的空间
2. 服务器为每个浏览器单独开辟一个session
3. 服务器根据浏览器发送过来的cookie，来确认当前浏览器使用哪个session

-   在WEB开发中，服务器可以为每个用户浏览器创建一个会话对象（session对象），注     意：一个浏览器独占一个session对象(默认情况下)。因此，在需要保存用户数据时，服   务器程序可以把用户数据写到用户浏览器独占的session中，当用户使用浏览器访问其     它程序时，其它程序可以从用户的session中取出该用户的数据，为用户服务。

-   Session和Cookie的主要区别在于：

  Cookie是把用户的数据写给用户的浏览器。

  Session技术把用户的数据写到用户独占的session中（服务器端）。

-  Session对象由服务器创建，开发人员可以调用request对象的getSession方法得到session对象。

Session没有构造函数，Session的获取依赖request的getSession()方法

Session是基于用户的请求，而把用户的重要信息在服务器端针对这个用户（浏览器）创建了一个容器。而这个Session容器是由web服务器(tomcat)帮助我们创建的，在程序中我们只能去获取到这个容器，然后给容器添加数据或者取出数据，或者删除数据，而我们是不能创建这个容器对象。

存数据：

setAttribute(String name , Object value)

取数据：

getAttribute(String name)

删数据：

removeAttribute(String name )

```java
		HttpSession session = request.getSession();
		session.setAttribute("username", "tom");
		String username = (String) session.getAttribute("username");
		System.out.println(username);
		session.removeAttribute("username");
		String username2 = (String) session.getAttribute("username");
		System.out.println(username2);
```

关闭浏览器之后，重新访问项目，被分配一个新的session对象，原因——用来寻找session对象的cookie已经不存在了，随着浏览器关闭消失了。

#### Session的生命周期

Session对象的创建时间：

当第一次调用request.getSession()的时候创建session容器.

​        如果第一次访问jsp页面,也会创建session容器

​        

#### Session的销毁时间：

1. （自动去死）Session在服务器的存活时间。Session对象在服务器内部有默认的存活的时间。一般默认是30分钟。如果在30分钟内，用户没有再对当前这个服务器中的资源进行任何的访问操作，这时只要时间到达30分钟，服务器会自动的销毁这个session。
2.  在程序执行中,手动销毁session容器, 使用 session.invalidate()
---
layout: post
title:  "请求参数乱码处理"
date:   2017-12-20
categories: web
tags: 请求参数乱码
---

* content
{:toc}
## 请求参数乱码处理

乱码产生的原因：

html使用utf-8编码----1---->Tomcat使用iso-8859-1解码，编解码不一致----2---->Servlet直接使用必然会产生乱码----3---->使用iso-8859-1编码----4---->使用utf-8解码

<!-- more -->

### Get 提交的中文乱码解决：

第一种方案：修改tomcat默认的编码方式（不推荐）

默认情况下，tomcat使用的的编码方式：iso-8859-1

修改tomcat下的conf/server.xml文件

找到如下代码：    

 &lt;Connector port="8080" protocol="HTTP/1.1" connectionTimeout="20000" redirectPort="8443" / &gt;

这段代码规定了Tomcat监听HTTP请求的端口号等信息。

可以在这里添加一个属性：URIEncoding，将该属性值设置为UTF-8，即可让Tomcat（默认ISO-8859-1编码）以UTF-8的编码处理get请求。

修改完成后：

 &lt;Connector port="8080"  protocol="HTTP/1.1" connectionTimeout="20000" redirectPort="8443" URIEncoding="UTF-8" / &gt;

#### 缺点:

方法不是很常用. 修改tomcat的servlet.xml

会很死板.如果两个项目: 一个是UTF-8,  另一个是GBK.

#### 第二种解决方案：先编码再解码

```java
	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
	/* //解决get方式的中文乱码
		//方式一：先编码，在解码
		String username = request.getParameter("username");
		//先使用iso-8859-1进行编码
		String encodeUsername = URLEncoder.encode(username, "sio-8859-1");
		//在使用utf-8进行解码
		username = URLDecoder.decode(username, "utf-8");*/
		
		//方式二：先编码，在解码
		String username = request.getParameter("username");
	/*	//先使用iso-8859-1进行编码
		byte[] bytes = username.getBytes("iso-8859-1");
		//在使用utf-8进行解码
		username = new String(bytes,"utf-8");*/
		
		username = new String(username.getBytes("iso-8859-1"),"utf-8");	
	}
```

### POST提交的中文乱码解决:

第一种解决方案 :先编码再解码

同get

第二种解决方案:设置请求编码(重点)

这种方式只对 请求体 有效，算是post的偷懒方式

```java
	public void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		request.setCharacterEncoding("utf-8");
        //解决输出的乱码问题
        response.setContentType("text/html;charset=utf-8");
		String username = request.getParameter("username");
	}
```





#### 



#### 

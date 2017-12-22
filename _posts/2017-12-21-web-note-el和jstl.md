---
layout: post
title:  "el表达式和jstl标签"
date:   2017-12-12
categories: web
tags: el表达式 jstl标签
---

* content
{:toc}
## el表达式

使用内置对象输出数据，代码太过繁琐。

EL全称：Expression Language

作用：代替jsp中脚本表达式的功能，简化对java代码的操作。

<!-- more -->

EL表达式的格式：${ 在容器中保存的数据的名称}request.setAtttribute(“username”,”zhangsan”)

如: ${requestScope.username} 

相当于 request.getAttribute(“username”) 

### 获得四个容器（域对象）的数据

```jsp
<%
	pageContext.setAttribute("name", "乾坤大挪移",pageContext.APPLICATION_SCOPE); 
	pageContext.setAttribute("name", "辟邪剑谱",pageContext.SESSION_SCOPE); 
	pageContext.setAttribute("name", "葵花宝典",pageContext.REQUEST_SCOPE); 
	pageContext.setAttribute("name", "九阳神功",pageContext.PAGE_SCOPE); 
%>
${name }<%-- 相当于<% pageContext.findAttribute(name) %> --%>
${ applicationScope.name}
${ sessionScope.name}
${ requestScope.name}
${ pageScope.name}
```

注：如果指定的key不存在会得到null，而使用el表达式取出的时候指定的key不存在，页面上什么都没有

### EL获取复杂数据

```jsp
<%@page import="top.forest.domain.Person"%>
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
  
  </head>
  
  <body>
	<%-- 演示EL获取复杂数据 --%>
	<%
		int[] arr = {1,2,3,4,6};
		pageContext.setAttribute("arr", arr);
	 %>
	${arr }
	${arr[1] }
	<hr>
	<%
		List list = new ArrayList();
		list.add("红烧肉");
		list.add("烤鱼");
		pageContext.setAttribute("list", list);
		
	 %>
	 ${list }
	  ${list[0] }
	 <hr>
	 <%
	 	Map map = new HashMap();
	 	map.put("tl1", "酱油");
	 	map.put("tl2", "鸡精");
	 	map.put("aa.bb.cc.dd", "沙爹");
	 	
	 	pageContext.setAttribute("map", map);
	 %>
	${map }
	${map.tl1}
	${map.tl2}
	${map["aa.bb.cc.dd"]}
	 <hr>
	<%
		Person p = new Person();
		p.setName("林青霞");
		p.setAge(14);
		pageContext.setAttribute("p", p);
	 %>
	 ${p }
	 ${p.age }
	 ${p.name }
	  ${p["age"] }
	 ${p["name"] }
	 <%-- 可以使用点的地方，都可以使用中括号获取数据 --%>
  </body>
</html>
```



#### el执行运算:

1、算术运算符：+  - /  % 

2、逻辑运算符：

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171221/8bbB22EAF0.png?imageslim)

3、比较运算符：

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171221/LDb62kjAkm.png?imageslim)

4、empty运算符：检查对象是否为null或“空”

​	对于自定义对象，检查是否为null

​	对于集合检查，是否为空（集合存在，但是没有数据）



### EL的11个内置对象

EL表达式它也有自己的内置对象可以直接在EL表达式中使用：

从不同的容器中取值

`pageScope`  `requestScope`  `sessionScope`  `applicationScope`

| param        | 获取用户提交的某个数据（请求参数）${param }          |
| ------------ | ----------------------------------- |
| paramValues  | 获取某个key对应的多个value值（获取页面中checkbox）   |
| header       | 获取请求头中的信息测试代码：${header }            |
| headerValues | 某个请求头中的多个value值                     |
| cookie       | 它获取到的一个cookie数组，获取所有的cookie数据       |
| pageContext  | 它就和JSP内置对象pageContext功能一致（获取其他内置对象） |
| initParam    | 获取的项目的全局配置参数                        |

1. EL底层还是java类
2. 通知服务器调用java类，准备配置文件
3. 需要在页面使用taglib指令导入函数



## JSTL的核心标签库

Jsp：html和java混杂，维护麻烦：

创建java标签，代替java代码，实现功能。



### c:if标签

作用：相当于java代码中if语句

使用c:if 标签，在JSP页面上可以完成if判断。注意：在JSTL核心标签库中没有c:else.只有if(){}结构 没有 if(){}else{}结构

属性:

Test：属性:判断是否执行标签内的内容。

Var：用来保存test属性的结果（，这个结果可以保存到指定的容器中。（如果没有指定容器，默认存入page容器中）

Scope：指定容器，保存数据

支持EL表达式

```jsp
<c:if test="false" var="aaa" scope="session">
   		测试if标签，var属性:${aaa }
   </c:if>
```

### `c:choose`  `c:when` `c:otherwise`

```jsp
c:choose c:whenc:otherwise 相当于：
if(){
 
}else if(){
 
} else if(){
 
}
。。。。。
else{
}
```

```jsp
<c:choose>标签它必须与<c:when>和<c:otherwise>标签一起使用
<c:choose>
    	<c:when test="${num==1 }">星期一</c:when>
    	<c:when test="${num==2 }">星期二</c:when>
    	<c:when test="${num==3 }">星期三</c:when>
    	<c:when test="${num==4 }">星期四</c:when>
    	<c:when test="${num==5 }">星期五</c:when>
    	<c:when test="${num==6 }">星期六</c:when>
    	<c:when test="${num==7 }">星期日</c:when>
 
    	<c:when test="${flag==1 }">白天</c:when>
    	<c:when test="${flag==2 }">黑夜</c:when>
    	<c:otherwise>参数不合法</c:otherwise>
</c:choose>
```

### c:set和c:out标签

c：set作用：它可以给某个容器中保存数据，或者去修改某个对象的属性值。

属性：

Var:声明了一个变量空间，用来存储数据(value属性的值)的

Value：要保存的数据

Scope：指定保存在那个容器中

Target：指定要修改的对象

Property：指定要修改的属性

```jsp
    <%-- var :声明一个变量空间，用来保存数据，（int i = 0;） --%>
    <c:set var="root" value="${pageContext.request.contextPath }" scope="session"/>
    ${sessionScope.root }
    <hr>
    <%
    	Person p = new Person();
    	pageContext.setAttribute("p", p);
     %>
     ${p }
    <c:set target="${p }" property="name" value="张三"  scope="session" var="pp"/>
    ${pp }
```

out 作用：它可以把数据输出到页面上，相当于JSP的内置对象out



### c:forEach标签：

c:forEach 循环的标签。 实现 java中for循环的功能

```jsp
 <%
    	Map map = new HashMap();
    	map.put("addr1", "东京");
    	map.put("addr2", "东莞");
    	map.put("addr3", "巴黎");
    	map.put("addr4", "东南亚");
    
    pageContext.setAttribute("map", map);
     %>
     <c:forEach items="${map }" var="entry">
    	${entry.value }
    	${entry.key }
    </c:forEach>
```


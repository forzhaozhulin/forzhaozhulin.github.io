---
layout: post
title:  "Servlet学习笔记---连接MySql"
date:   2016-03-9 09:43:54
categories: JavaWeb
tags: Servlet MySql
author: 薛彬
---

* content
{:toc}

最近帮导师完成的一个项目是在做数据可视化，所以就需要用到servlet连接MySql读取数据。




## 连接数据库

首先我们需要建立数据库的连接。

```java
public class DBManager {
    //使用tomcat配置的数据库连接池来获取连接
    static public Connection get_connection()
    {
        Connection conn = null;
        try {      
            //初始化查找命名空间
            Context ctx = new InitialContext();
            DataSource ds = (DataSource)ctx.lookup("java:comp/env/jdbc/dataminningdb"); 
            conn = ds.getConnection();
        } catch (Exception e) {
            //System.out.println(e);
            e.printStackTrace();
        }
        return conn;
    }
}
```

连接完数据库之后我们需要读取数据库中的数据。

```java
Connection con = null; Statement statement = null; ResultSet rs = null;
    try {
        con = DBManager.get_connection();
        String sql = String.format("select * from %s", table_name);
        statement = con.createStatement();
        rs = statement.executeQuery(sql);
        while(rs.next()) {
        }
    }catch(Exception e) { }
    finally {
        try { if(rs != null) rs.close(); } catch(Exception e) {}
        try { if(statement != null) statement.close(); } catch(Exception e) {}
        try { if(con != null) con.close(); } catch(Exception e){}
    }
```

## 写servlet

我们都知道servlet中其实就是对doGet()/doPost()方法进行重写，实现所需要的功能。

```java
public class PopulationQuery extends HttpServlet {
	 protected void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        response.setContentType("application/json; charset=utf-8");
        try {
            }
        }catch(Exception e) {}
    }
}
```

## 配置context.xml

需要在这个文件中配置有关数据库的一些参数。

```xml
name="jdbc/dataminningdb"
username="xb"
password=""
driverClassName="com.mysql.jdbc.Driver"
validationQuery='select 1'
url="jdbc:mysql://localhost:3306/dataminning"/>
```

## 配置web.xml

在这个文件中配置servlet

```xml
<servlet>
        <servlet-name>PopulationQuery</servlet-name>
        <servlet-class>dataminning.PopulationQuery</servlet-class>
</servlet>
<servlet-mapping>
        <servlet-name>PopulationQuery</servlet-name>
        <url-pattern>/PopulationQuery</url-pattern>
</servlet-mapping>
```

这是配置一个名为PopulationQuery的servlet，指向dataminning.PopulationQuery这个class，就会执行这个文件中的doGet/doPost方法。


>关于web.xml这个文件中出现的

用户通过URL地址访问web应用资源，所以如果servlet想要被外部访问，就一定要将servlet文件映射到一个URL地址上面。经常会在web.xml中这样写。

```java
<servlet>
    <servlet-name>HelloServlet</servlet-name>
    <servlet-class>src.HelloServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>HelloServlet</servlet-name>
    <url-pattern>/HelloServlet</url-pattern>
</servlet-mapping>
```

- servlet-name:servlet的名称
- serlvet-class:servlet对应的java类名
- url-pattern:servlet的对外访问路径，也就是用户可以在游览器上看到的路径

常见的元素还有：

```java
	<init-param></init-param>//这是用来定义参数
```



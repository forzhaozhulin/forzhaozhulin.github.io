---
layout: post
title:  "大数据可视化平台（前后端）"
date:   2016-03-19 19:14:54
categories: 个人项目
tags: Java Servlet MySql
author: 薛彬
---

* content
{:toc}

嗯，这算是研究生入学后接触的第一个项目吧，其实就是一个demo系统吧，用来做数据可视化。







>更新至热力图。

## 系统设计

### 整体框架

本系统是采用了三层体系结构进行搭建。作为底层的数据层是由MySQL数据库组成，进行数据存储、管理和维护，并在接收到查询指令后执行操作。作为中间层的服务器层是采用Tomcat服务器和Servlet语言，负责处理用户的访问请求，通过Servlet等对数据库进行访问。用户层就是用户的浏览器，通过HTML5+CSS+JavaScript设计一个页面，嵌入百度地图JavaScript API和Echarts，主要实现地图加载、地图浏览、数据图表展示等功能，并且发出数据库查询请求给服务端。

![](http://i.imgur.com/heyAprd.png)

### 数据库

本系统的数据库采用的是MySQL数据库，原始数据经过R语言数据统计处理，导出一个csv文件，将其导入MySQL数据库中。其中包括目标地区按小区划分的地域表以及人口信息表。比如需要直观的展示目标地区小区划分情况，就需要建立相关的数据表，其中包含编号、名称、中心纬度、中心经度、人口数量等信息，通过百度地图JavaScript API可以在地图上利用这些数据添加轮廓，构成一幅地区小区分布图。

![](http://i.imgur.com/0kdGc0a.png)

### 服务端设计

本系统的服务端采用的是Apache Tomcat服务器搭建，采用JDBC(Java Data Base Connectivity)通过Tomcat配置一个数据库连接池，获取MySQL数据库的连接，这是服务器与数据库实现通信最有效的方式。Servlet需要先进行初始化，获取配置信息，然后创建Servlet对象，调用service方法，根据提交的Get方式，选择经过重写的doGet方法并执行，获取数据库中的数据，保存为Json格式的数据然后返回用户端。在服务端中，客户端（游览器）可以向服务端发出Request请求，并将接受一个Response。

### 前端页面设计

本系统的用户端是一个Web站点，通过HTML+CSS+JavaScript等web技术进行前端网页设计。HTML称为超文本标记语言，是为“网页创建和其他可在网页浏览器中看到的信息”设计的一中标记语言，用来结构化信息；CSS称为层叠样式表，主要是为结构化文档（本系统中即是HTML文档）添加样式；JavaScript是一种标准化的脚本语言，在本系统中主要是应用于对浏览器事件作出相应，其Ajax技术是本系统的一个核心技术，Ajax即异步的JavaScript与XML技术，是一套综合了多项技术的浏览器端网页开发技术，通过Ajax技术可以实现在不重复加载网页的基础上对网页上的地图进行动态刷新。

## 前期工作

### 百度地图JavaScript API

#### API的获取

>百度地图JavaScript API是一套由JavaScript语言编写的应用程序接口，可帮助您在网站中构建功能丰富、交互性强的地图应用，支持PC端和移动端基于浏览器的地图应用开发，且支持HTML5特性的地图开发。

首先我们需要在HTML页面上加入语句

	<script type="text/javascript" src="http://api.map.baidu.com/api?v=2.0&ak=密钥"></script>

这样就算是引用了API了，其中的密钥是需要在[百度地图LBS开放平台](http://lbsyun.baidu.com/apiconsole/key?application=key)申请。

## 具体实现

### 热力图

本系统中的热力图是用的百度地图JavaScript API中。

#### 准备数据文件

现在我们手中只有csv文件，我们需要将其导入数据库中。

地图定位的csv文件导入mysql（由于数据是私密的，所以给胡乱打码了一下，只要关心表的列名就好了）：

![](http://i.imgur.com/bkk7GDt.png)

每个地点对应的人口数：

![](http://i.imgur.com/qvSNXr3.png)

第二个表格只是给了一个地点代码，需要通过第一个表格得到这个地点的经纬度，所以我们要创建一个视图：

```sql
CREATE VIEW people_count_view AS SELECT s.smid, s.count,i.coding, i.center_lng, i.center_lat FROM people_count s, polygons i WHERE i.smid=s.smid;
```

得到如下的视图：

![](http://i.imgur.com/djBua0Q.png)

这样我们就得到了每个地点对应的经纬度以及人数，这就有了热力图的数据。

#### servlet

servlet在这个项目中其实就是一个作用，接到浏览器的request然后向mysql数据库发出数据请求，得到数据后返回给浏览器。

```java
public class PopulationQuery extends HttpServlet {
    @Override
    public void init() throws ServletException
    {
    }
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        response.setContentType("application/json; charset=utf-8");
        try {
        }catch(Exception e) {}
    }
}
```

所以我们也只要重写一下doGet()方法就好了，在方法中编写数据的请求。

#### 数据库

##### 创建数据库连接

在这里我们因为要用servlet请求数据库，所以使用tomcat配置的数据库连接池来获取连接：

```java
public class DBManager {
    //使用tomcat配置的数据库连接池来获取连接
    static public Connection get_connection()
    {
        Connection conn = null;
        try {      
            //初始化查找命名空间
            Context ctx = new InitialContext();  
            //参数java:comp/jdbc/learning_db为数据源和JNDI绑定的名字
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

这段代码网上都有，只要修改参数就可以使用了。还需在context.xml中修改对应的数据源。

##### 读取数据库

这里大家可以自己查到JDBC的资料看看，我只列出一段代码：

```java
Connection con = null; 
Statement statement = null; ResultSet rs = null;
     	try {
            con = DBManager.get_connection();
            String sql = String.format("select * from %s where coding<>6", table_names.polygon_table);
            statement = con.createStatement();
            rs = statement.executeQuery(sql);
            while(rs.next()) {
                int id = rs.getInt("smid");
                int p_id = rs.getInt("parent_id");
                int coding = rs.getInt("coding");
                double center_lng = rs.getDouble("center_lng");
                double center_lat = rs.getDouble("center_lat");
                String name = rs.getString("name");
				/*这里开始省略了*/
            }
        }catch(Exception e) { }
        finally {
            try { if(rs != null) rs.close(); } catch(Exception e) {}
            try { if(statement != null) statement.close(); } catch(Exception e) {}
            try { if(con != null) con.close(); } catch(Exception e){}
        }
```

这样就完成了数据库读取的操作，我们现在需要将其通过servlet返回到页面中。（当然，步骤其实是浏览器先发出请求给servlet，然后servlet请求数据。）

#### 热力图代码

##### 加载初始化地图

在实现地图展示功能之前，需要在页面上加载初始地图，初始化代码如下：

```javascript
var map = new BMap.Map("baidu_map"); 
map.centerAndZoom(newBMap.Point(116.404, 39.915), 8); 
map.enableScrollWheelZoom(); 
var top_left_control = new BMap.ScaleControl({anchor: BMAP_ANCHOR_TOP_LEFT});
var top_left_navigation = new BMap.NavigationControl();add_control();
function add_control() 
{ 
	map.addControl(top_left_control); 
	map.addControl(top_left_navigation); 
}
```
##### 热力图

```javascript
function PopulationHeatmap(map, population) {
    var heatmap = map;
    var heatmapOverlay=null;
    var population_infos = population;
    gradient = {0: 'rgb(102, 255, 0)', .5: 'rgb(255, 170, 0)', 1: 'rgb(255, 0, 0)'};
    if(!heatmapOverlay){
        heatmapOverlay = new BMapLib.HeatmapOverlay({"radius": 20});
    }
    heatmap.addOverlay(heatmapOverlay);
    var ds = getDataSet(population_infos);
    heatmapOverlay.setDataSet(ds);
    heatmapOverlay.setOptions({"gradient:": gradient});
    heatmapOverlay.show();
    function getDataSet(population_infos) {
        var max_val = 0;
        var ds = [];
        for (var i = 0; i < population_infos.length; ++i) {
            if (population_infos[i].coding == 6) {
                ds.push(population_infos[i]);
                if (population_infos[i].count > max_val)
                    max_val = population_infos[i].count;
            }
        }
        return {data: ds, max: max_val};
    }
}
```

这就是结果：

![](http://i.imgur.com/IQTtoy3.jpg)


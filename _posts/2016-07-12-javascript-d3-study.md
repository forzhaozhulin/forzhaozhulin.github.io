---
layout: post
title:  "D3笔记"
date:   2016-07-12 15:12:00
categories: 前端
tags: D3 JavaScript
author: 薛彬
---

* content
{:toc}





## 入门

### 元素操作

#### 如何选择元素

- d3.select()：是选择所有指定元素的第一个
- d3.selectAll()：是选择指定元素的全部

选择全部：

```html
<!DOCTYPE html>
<html lang="zh-CN">
    <head> 
        <meta charset="utf-8"> 
        <title>HelloWorld</title> 
        <script src="http://d3js.org/d3.v3.min.js" charset="utf-8"></script> 
    </head> 
    <body> 
        <p>Nokia</p>
        <p>Apple</p>
        <p>Moto</p>
        <script>  
            var str = "MobilePhone";
            var body = d3.select("body");
            var p = body.selectAll("p");
            p.datum(str);
            p.text(function(d, i){
                return "第 "+ i + " 个元素绑定的数据是 " + d;
            });
        </script> 
    </body> 
</html>
```

选择一个：

```html
    <p id="id">hhh</p>
    <script> 
        var p=bodu.select("#id");
    </script>  
```

选择多个：

```html
    <p class="id">first</p>
    <p class="id">second</p>
    <script> 
        var p=bodu.select(".id");
    </script>  
```

#### 如何绑定元素

- datum()：绑定一个数据到选择集上
- data()：绑定一个数组到选择集上，数组的各项值分别与选择集的各元素绑定

使用被绑定的数据大多数用以下形式：定义一个无名函数function（d,i），在函数体中使用d和i。

```html
<!DOCTYPE html>
<html lang="zh-CN">
    <head> 
        <meta charset="utf-8"> 
        <title>HelloWorld</title> 
        <script src="http://d3js.org/d3.v3.min.js" charset="utf-8"></script> 
    </head> 
    <body> 
        <p>Nokia</p>
        <p>Apple</p>
        <p>Moto</p>
        <script>  
        var dataset = ["I like Nokia","I like Apple","I like Moto"];
        var body = d3.select("body");
        var p = body.selectAll("p");
        p.data(dataset)
          .text(function(d, i){
              return d;
          });
        </script> 
    </body> 
</html>
```

#### 插入元素

- append()：在选择集末尾插入元素
- insert()：在选择集前面插入元素

#### 删除元素

- remove()

### 添加画布

添加SVG画布：

```javascript
var width = 300;  //画布的宽度
var height = 300;   //画布的高度
var svg = d3.select("body")     //选择文档中的body元素
    .append("svg")          //添加一个svg元素
    .attr("width", width)       //设定宽度
    .attr("height", height);    //设定高度
```

### 比例尺

- 线性比例尺：d3.scale.linear() 
- 序数比例尺：d3.scale.ordinal()

添加比例尺

```javascript
var dataset = [ 2.5 , 2.1 , 1.7 , 1.3 , 0.9 ];
var linear = d3.scale.linear()
        .domain([0, d3.max(dataset)])
        .range([0, 250]);
svg.selectAll("rect")
    .data(dataset)
	//省略....
    .attr("width",function(d){
         return linear(d);   //在这里用比例尺
    })
```

### 坐标轴

#### 绘制方法

1. 定义坐标轴：`var axis=d3.svg.axis()`
2. 在svg中添加一个包含坐标轴各元素的g元素：`var gAxis=svg.append("g")`
3. 在gAxis中绘制坐标轴：`axis(gAxis)`

#### 设置

- axis.orient():设置坐标轴的方向，有`"top"`,`"bottom"`,`"left"`,`"right"`四个值
- axis.ticks():设置坐标轴的分割数，默认为10。若设为5,则刻度数量为6,有5段间隔
- axis.tickValues():设置坐标轴的制定刻度，如：[2,3,4,5],则只有这几个数字上会有刻度


## 绘制

### 线段

类似svg的`line`形式添加：

```javascript
svg.append("line")
    .attr("x1",20)
    .attr("y1",20)
    .attr("x2",300)
    .attr("y2",100)
```

类似svg的`path`形式添加：

```javascript
svg.append("path")
    .attr("d","M20,20L300,100")
```

### 区域生成器

`d3.svg.area()`

数据访问器有六个：x(),x0(),x1(),y(),y0(),y1()，不用全部用。其中：

- x：各段的x坐标
- y0：区域的下限坐标
- y1：区域的上限坐标

### 弧生成器

访问器：

- innerRadius():内半径访问器
- outerRadius():外半径访问器
- startAngle():起始角度访问器，单位：弧度
- endAngle():终止角度访问器，单位：弧度

起始角度为0时是在时钟零点的位置，终止角度是按顺时针旋转的。


## 导入

### 导入json

```javascript
d3.json("demo.json",function(error,data){
    console.log(data);
})
```

### 导入csv

```javascript
d3.csv("demo.json",function(error,data){
    console.log(data);
})
```
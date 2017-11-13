---
layout: post
title:  "D3.js实现最简单柱状图的例子"
date:   2016-07-12 16:58:00
categories: 前端
tags: D3 JavaScript
author: 薛彬
---

* content
{:toc}

今天开始学习D3.js，这个例子是通过d3.js画一个简单的柱状图。





先把效果图放出来：

![](http://i.imgur.com/P3wfTZp.png)

具备了一个柱状图的基础元素：柱形，坐标轴，刻度，数值等。

不得不说，d3.js比直接用的echarts更麻烦，但是确实更自由。

来看看如何实现吧。

```javascript
//确定画布的大小
var width = 400;
var height = 400;
//在 body 里添加一个 SVG 画布	
var svg = d3.select("body")
        .append("svg")
        .attr("width", width)
        .attr("height", height);
//定义画布周围空白的地方
var padding = {left: 30, right: 30, top: 20, bottom: 20};
//定义一个数组
var dataset = [10, 20, 30, 40, 30, 20, 10, 5];
//x轴的比例尺
var xScale = d3.scale.ordinal()
        .domain(d3.range(dataset.length))
        .rangeRoundBands([0, width - padding.left - padding.right]);
//y轴的比例尺
var yScale = d3.scale.linear()
        .domain([0, d3.max(dataset)])
        .range([height - padding.top - padding.bottom, 0]);
//定义x轴
var xAxis = d3.svg.axis()
        .scale(xScale)
        .orient("bottom");
//定义y轴
var yAxis = d3.svg.axis()
        .scale(yScale)
        .orient("left");
//矩形之间的空白
var rectPadding = 4;
//添加矩形元素
var rects = svg.selectAll(".MyRect")
        .data(dataset)
        .enter()
        .append("rect")
        .attr("class", "MyRect")
        .attr("fill", "steelblue")
        .attr("transform", "translate(" + padding.left + "," + padding.top + ")")
        .attr("x", function (d, i) {
            return xScale(i) + rectPadding / 2;
        })
        .attr("y", function (d) {
            return yScale(d);
        })
        .attr("width", xScale.rangeBand() - rectPadding)
        .attr("height", function (d) {
            return height - padding.top - padding.bottom - yScale(d);
        });
//添加文字元素
var texts = svg.selectAll(".MyText")
        .data(dataset)
        .enter()
        .append("text")
        .attr("class", "MyText")
        .attr("fill", "white")
        .attr("text-anchor", "middle")
        .attr("transform", "translate(" + padding.left + "," + padding.top + ")")
        .attr("x", function (d, i) {
            return xScale(i) + rectPadding / 2;
        })
        .attr("y", function (d) {
            return yScale(d);
        })
        .attr("dx", function () {
            return (xScale.rangeBand() - rectPadding) / 2;
        })
        .attr("dy", function (d) {
            return 20;
        })
        .text(function (d) {
            return d;
        });
//添加x轴
svg.append("g")
        .attr("class", "axis")
        .attr("transform", "translate(" + padding.left + "," + (height - padding.bottom) + ")")
        .call(xAxis);
//添加y轴
svg.append("g")
        .attr("class", "axis")
        .attr("transform", "translate(" + padding.left + "," + padding.top + ")")
        .call(yAxis);
```
---
layout: post
title:  "CSS"
date:   2017-12-18
categories: web
tags: CSS 
---

* content
{:toc}
# CSS

css通常称为css样式或层叠样式表。

<!-- more -->

一般标签都可以添加一个style属性来书写样式。

## CSS使用的基本语法：

```css
<标签名 style="样式名1 : 样式值1；样式名2 : 样式值2；…" ></标签名>
     属性名 和 属性值 之间通过 ： 来隔开
     多个样式之间使用 ; 隔开
     一般复合样式 (border, font…) 值是多个，而多个值使
     用空格隔开。例如： border : 1px solid #000；
```

这种方式不好,HTML和css结合代码太多难以维护,冗余代码过多。

### 方式二：在head标签内使用style标签设置

```html
<head>
<style type=”text/css”>
标签名称 ｛
样式名1：样式值1；
样式名2：样式值2；
｝
</style>
</head>
```

###方式三：在head标签中的style标签使用@import：

```html
<style type="text/css">
@import url("css文件的地址");
</style>
```

###  方式四：在head标签中使用link标签（外部）：

```css
<link rel="stylesheet" type="text/css" href="css文件路径" />
```



## 选择器:

### 标签名选择器

```html
标签名称｛
	css代码;
｝
```

### ID选择器：

作用某一个为特定的标签上

```html
#选择器的名称 ｛
	css代码；
｝	


<标签名 id=” 选择器的名称” />
```

### class选择器

作用在一组标签之上

```html
.class选择器的名称 ｛
	css代码；
｝

<标签名 class=”xxxx” />
```

### 组合选择器：

应用场景：如果页面不同的选择器想使用相同的css样式

多个选择器一逗号（,）隔开

```html
选择器1, 选择器2... {
    css代码
}
```

### 关联选择器：

```html
标签名 后代标签名 ｛
    css代码；
｝
```

### 锚伪类选择器 ：

```html
选中的标签 ： 伪元素名称 {
   css代码
}
```

#### 4种锚伪类选择器：

坐在超链接标签<a>上的

* link        某个html标签未被点击之前的状态
* visited     鼠标点击之后，松开了
* hover     鼠标悬浮式
* active     鼠标点击 但没有松开

```html
<style type="text/css">
			a:link {
				color: yellow;
				text-decoration: none;
			}
			a:visited {
				color: blue;
			}
			a:hover {
				color: red;
				font-size: 50px;
			}
			a:active {
				color: green;
			}
</style>
```

### 属性选择器:

属性选择器是在原有选择器的基础上，通过标签的属性，再次对标签进行筛选

```html
选择器名[属性名="属性值"] {
	css样式；
}
```

## css的常用样式：

### 边框和尺寸：border width height

border ：设置边框的样式

​	格式：宽度 样式 颜色

​	例如：style=”border:1px solid #f00”  ，1像素实边红色。

​	样式取值：solid 实线，none 无边，dashed虚线 等

width、height：用于设置标签的宽度、高度。

### display:

HTML提供丰富的标签，这些标签被定义成了不同的类型，一般分为：块标签和行内标签。

块标签：以区域块方式出现。每个块标签独自占据一整行或多整行。

​	常见的块元素：<h1>、<div>、<ul>等

​	默认可以设置高度和宽度

行内元素：不必在新的一行开始，同时也不强迫其他元素在新的一行显示。

​	常见的行内元素：<span>、<a> 等

​	设置高度和宽度无效

在开发中，希望行内元素具有块元素的特性，需要使用display属性将行内元素转换成块级元素 

语法结构：

​	选择器 {display : 属性值 }

常用的属性值：

* inline：此元素将显示为行内元素（行内元素默认的display属性值）
* block：此元素将显为块元素（块元素默认的display属性值）
* none：此元素将被隐藏，不显示，也不占用页面空间。

```html
<html>
  <head>
  <meta charset="UTF-8">
  <title></title>
  <style type="text/css">
    span {
    border :1px solid #f00;
    width:100px;
    height:50px;
  }
  </style>
  </head>
  <body>
    <!--默认显示一行，高宽没有作用-->
    <span>普通sapn1</span>
    <span>普通sapn2</span>
    <!--每一行显示，高宽有作用-->
    <span style="display: block;">转成块的sapn1</span>
    <span style="display: block;">转成块的sapn2</span>
  </body>
</html>
```

### 字体：color、line-height:

语法格式：

​	color：     颜色    字体颜色

​	line-height:  行高    设置行高

注：给元素设置行高一般设置为当前父容器的高度以便于让当前的元素在父元素中居中显示

### 背景：background-color，background-image,background-repeat

background-color： 颜色;  设置元素的背景颜色

background-image : url(图片的路径地址); 

background-repeat: 背景平铺方式; 

背景平铺方式取值：

​	no-repeat： 不平铺

​	repeat-x ： 横向平铺

​	repeat-y ： 纵向平铺

注意：图片默认是平铺满整个盒子的

### 布局：float、clear

通常默认的排版方式，将页面中的元素从上到下一一罗列，而实际开发中，需要左右方式进行排版，就需要使用浮动

语法结构：

​		选择器 {

​			float : 属性值;

​		}

常用属性值：

​	left：元素向左浮动

​	right：元素向右浮动

​	none：元素不浮动（默认值）



### 使用clear属性进行清除浮动。

​		选择器 { 

​			clear : 属性值; 

​		}

常用属性值：

​	left：不允许左侧有浮动元素（清除左侧浮动的影响）

​	right：不允许右侧有浮动元素（清除右侧浮动的影响）

​	both：同时清除左右两侧浮动的影响



## 盒模型

所有HTML元素可以看作盒子.每个盒子都由元素的内容、内边距（padding）、边框（border）和外边距（margin）组成。

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171218/0ijhDbgf4H.png?imageslim)

- **Margin(外边距)** - 清除边框外的区域，外边距是透明的。
- **Border(边框)** - 围绕在内边距和内容外的边框。
- **Padding(内边距)** - 清除内容周围的区域，内边距是透明的。
- **Content(内容)** - 盒子的内容，显示文本和图像。

盒模型对css来说非常重要。要想设计一个好的页面，离不开盒模型。

以后有时间再深入学习。










---
layout: post
title:  "百度Web前端技术学院--水平垂直居中"
date:   2016-03-18 20:12:00
categories: 前端
tags: HTML CSS
author: 薛彬
---

* content
{:toc}




## 任务

- 实现如[示例图（点击查看）](http://7xrp04.com1.z0.glb.clouddn.com/task_1_4_1.png)的效果。
- 灰色元素水平垂直居中，有两个四分之一圆位于其左上角和右下角。

## 实现

### demo
[demo](http://axuebin.com/marginAutoDemo)

### 源码

css代码：

```css
   .container{
        width:400px;
        height: 200px;
        background-color: #ccc;
        position: absolute;
        top:0;
        bottom: 0;
        left: 0;
        right: 0;
        margin:auto;
        overflow:hidden;
    }
    /*第一种方式实现圆角*/
    .left-circle{
        background-color: #fc0;
        width: 50px;
        height: 50px;
        border-bottom-right-radius:50px;
    }
    /*第二种方式实现圆角,其实是画一个圆，然后遮住3/4，得要在container中加入overflow:hidden了*/
    .right-circle{
        background-color: #fc0;
        width: 100px;
        height: 100px;
        border-radius:50px;
        position: absolute;
        bottom:-50px;
        right:-50px;
    }
```

## 水平垂直居中的实现

### 绝对定位的方式

```css
	.container {
    	width: 400px; 
		height: 200px;
    	position: absolute; 
		left: 50%; 
		top: 50%;
    	margin-top: -200px;    /* 高度的一半 */
    	margin-left: -300px;    /* 宽度的一半 */
	}
```

这个方法等于是在手动计算margin的值了，如果不知道div的宽度和高度，我们还得借助js的document来获得，就显得麻烦了。

### margin:auto的方式

我用的就是这种方式，通过设置margin:auto来实现居中效果。

这里面最关键的就是要让top/bottom/left/right都得设置为0。

其实这里我不明白的是为什么我单单写了一个`margin:auto`只是水平居中，根据margin值得设置方法，`margin:auto`难道不是`margin: auto auto auto auto`的意思吗？

这样设置了之后结果变成了这样：

![](http://i.imgur.com/wTu1bqu.png)

强行卖了一波萌~

后来才知道原来`margin-top:auto`和`margin-bottom:auto`的计算值是0，所以`margin:auto`只是水平居中了。

然后要实现水平垂直居中我们只能这样了。先把top/bottom/left/right都设置成0,然后再设置`margin:auto`就可以实现了：

![](http://i.imgur.com/nfFBVOm.png)

这里可以参考两个地方：

[为什么「margin:auto」可以让块级元素水平居中？](https://www.zhihu.com/question/21644198)

[小tip: margin:auto实现绝对定位元素的水平垂直居中](http://www.zhangxinxu.com/wordpress/2013/11/margin-auto-absolute-%E7%BB%9D%E5%AF%B9%E5%AE%9A%E4%BD%8D-%E6%B0%B4%E5%B9%B3%E5%9E%82%E7%9B%B4%E5%B1%85%E4%B8%AD/)

## 笔记

#### `position`

position是决定了元素将如何定位的属性。

|值|描述|
|---|:---|
|static|这是默认值。相当于没有定位。|
|relative|相对定位，相对于正常位置来定位。|
|absolute|绝对定位，相对于**最近一级**的不是static的父元素定位。|
|fixed|绝对定位，相对于**浏览器窗口**来定位。即便页面滚动，它还是会在相同的位置。|
|inherit|从父元素继承。|

#### `top、bottom、left、right`

该属性规定元素的顶部边缘。分别定义了一个定位元素的上下左右外边距边界与其包含块上下左右边界之间的偏移。

注意：对于相对定义元素，如果top和bottom都是auto，则计算值相当于0。

#### `border-bottom-right-radius`

这条属性是CSS3新添加的属性。是用来定义右下角边框的形状的。

这条属性可以有两个值，也就是：

`border-bottom-right-radius: length|% [length|%];`

第一个值是水平半径，第二个值是垂直半径。如果忽略了第二个值，则复制第一个值。

注意：如果长度为0，则边角就是一个正方形；如果只有一个值，则是圆形。

#### `border-radius`

这条属性是CSS3新添加的属性。用来设置四个border-*-radius属性，顾名思义就有四个值了。

这样一条`border-radius=50px`就相当于：

```css
	border-top-left-radius:50px;
	border-top-right-radius:50px;
	border-bottom-right-radius:50px;
	border-bottom-left-radius:50px;
```

## 参考资料

- [MDN：position](https://developer.mozilla.org/zh-CN/docs/Web/CSS/position)：了解 CSS position 属性的基本知识
- [MDN：float](https://developer.mozilla.org/en-US/docs/Web/CSS/float)：了解 CSS float 属性的基本知识
- [Learn CSS Positioning in Ten Steps](http://www.barelyfitz.com/screencast/html-training/css/positioning/)：通过具体的例子熟悉 position 属性
- [清除浮动（clearfix hack）](http://zh.learnlayout.com/clearfix.html)：清除浮动是什么，如何简单地清除浮动
- [StackOverflow](http://stackoverflow.com/questions/211383/which-method-of-clearfix-is-best)：Which method of ‘clearfix’ is best?：清除浮动黑科技完整解读


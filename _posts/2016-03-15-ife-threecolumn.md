---
layout: post
title:  "百度Web前端技术学院--三栏式布局"
date:   2016-03-15 14:12:00
categories: 前端
tags: HTML CSS
author: 薛彬
---

* content
{:toc}




## 任务

- 使用 HTML 与 CSS 按照[示例图（点击查看）](http://7xrp04.com1.z0.glb.clouddn.com/task_1_3_1.png)实现三栏式布局。
- 左右两栏宽度固定，中间一栏根据父元素宽度填充满，最外面的框应理解为浏览器。背景色为 #eee 区域的高度取决于三个子元素中最高的高度。

## 左右固定中间自适应布局

### 源码

html代码：

```html
<html>
<head>
    <meta charset="utf-8">
    <title>三栏布局</title>
</head>
<body>
    <div class="container">
        <div class="left">
            <img id="grouplogo" src="https://r11.fodey.com/2410/d056a0d89d2946dcafcf0751f7225e0e.1.gif" border: 0 width="80" height="80" >
            <div id="groupname">webGroup</div>
        </div>
        <div class="right">
            <img id="member1" class="memberlogo" src="img/code.png" title="张权">
            <img id="member2" class="memberlogo" src="img/touxiang.png" title="薛彬">
            <img id="member3" class="memberlogo" src="img/logo.jpg" title="农振立">
            <img id="member4" class="memberlogo" src="img/yangchenhao.png" title="杨晨昊">
            <img id="member5" class="memberlogo" src="img/wechat.jpg" title="王志文">
        </div>
        <div class="center">
            <h3 style="color:red;">webGroup</h3>
            <p>
                webGroup团队是百度前端学院2016级学院的一个小团队，由来自各地的前端兴趣小伙伴组成~成员共有五名，都想在此次学院中有所收获！
            </p>
            <strong><a href="http://axuebin.com/webGroup" target="_blank">团队主页(可以点一下哦~)</a></strong>
            <p><strong>张权:</strong>来自中国地质大学（武汉）计算机学院。作为程序猿，具备程序猿应有的气质和帅气！</p>
            <p><strong>薛彬:</strong>来自中国计量学院信息工程学院。喜欢看书和实践，热衷于解决具有挑战性的问题。<a href="http://axuebin.com" target="_blank">个人博客：http://axuebin.com</a></p>
            <p><strong>农振立:</strong>Believe that practice makes perfect.代码是敲出来的：坚信熟能生巧!<a href="mailto:nong99@outlook.com">   有事没事给我发邮件吧~</a></p>
            <p><strong>杨晨昊:</strong>一个在自学编程之路上努力探索的大四营销狗.Stay hungry, Stay foolish! <a href="http://chenhao-yang.github.io/" target="_blank">个人博客：http://chenhao-yang.github.io/</a></p>
            <p><strong>王治文:</strong>似乎是一名技术渣，从前幻想过做很多很多事情，但是都没有坚持下来，现在遇见了前端，真心喜欢并且希望能够坚持下去。</p>
        </div>
    </div>
</body>
</html>
```

附上css：

```css
    div{box-sizing: border-box;}
    .container,.left,.right,.center{padding:20px;border:1px solid #999;}
    .container {background-color: #eee;height: 580px;}
    .left {width: 200px;float: left;background-color: #fff;}
    .right {width: 120px;float: right;background-color: #fff;}
    .center {height: 400px;margin-left:220px;margin-right:140px;background-color: #fff;overflow:scroll}
    #grouplogo {width: 80px;height: 80px;float: left;}
    #groupname {font-weight: bold;color:darkblue;float: left;text-indent: 10px;}
    .memberlogo {width: 80px;height: 80px;border:1px solid #999;}
    #member1,#member2,#member3,#member4{margin-bottom:15px;}
    .center p{text-indent: 1cm; }
    .center a{text-decoration: none;}
```

## 笔记

### html

这里我们用到了“左右侧浮动，中间margin的方法”来实现左右固定中间自适应的效果。

这里记录一下用到的html标签：

#### `<img></img>`

这是html中的插入图片的标签。我在这个标签中使用了两个属性，分别是src和title属性。src属性顾名思义就是源文件了，而title属性主要是为图片提供描述性文字，在鼠标停留在图片上的时候，这个文字就会出现。

#### `<a href="" target="_blank"></a>`

`<a>`标签的href属性是用来指定超链接目标的URL。这个URL可以是一个网页，一个图片甚至是一段Javascript代码段。

我们可以为它添加`target="_blank"`属性。如果有了这个属性，在点击链接的时候就会在新窗口显示内容。

另外，我们还可以让这个链接的不同状态以不同方式显示：

```css
a:link {color: #FF0000}		/* 未访问的链接 */
a:visited {color: #00FF00}	/* 已访问的链接 */
a:hover {color: #FF00FF}	/* 鼠标移动到链接上 */
a:active {color: #0000FF}	/* 选定的链接 */
/*tips:
1.a:hover 必须被置于 a:link 和 a:visited 之后，才是有效的。
2.a:active 必须被置于 a:hover 之后，才是有效的。
*/
```

### css

这里记录一下用到的css属性：

#### `float`

float 属性定义元素在哪个方向浮动。以往这个属性总应用于图像，使文本围绕在图像周围，不过在 CSS 中，任何元素都可以浮动。浮动元素会生成一个块级框，而不论它本身是何种元素。

它的值有：left（左）,right（右）,none（无）,inherit（父元素继承）。

我们来看看三种情况下的：

![](http://i.imgur.com/9QaM9p0.png)

这个是没有添加float属性的。

![](http://i.imgur.com/t04XlO8.png)

这个是添加了`.first{float:left;}.second{float:left;}`的效果，这时候第二个div就是从左到右紧贴第一个div。

![](http://i.imgur.com/IGQW07g.png)

再看看这第三种情况，这是`.first{float:left;}.second{float:right;}`的效果，此时第一个div是靠左浮动，而第二个div是靠右浮动。

#### `margin`

margin 简写属性在一个声明中设置所有外边距属性。该属性可以有1到4个值：

```css
margin:10px 5px 15px 20px;/*上10 右5 下15 左20，也就是顺时针*/
margin:10px 5px 15px;/*上10 左右5 下15*/
margin:10px 5px;/*上下10 左右5*/
margin:10px;/*上下左右都是10*/
```

#### `overflow`

overflow 属性规定当内容溢出元素框时发生的事情。它有以下几个值：

|值|描述|
|---|:---|
|visible|会溢出元素框。|
|hidden|溢出的元素就看不见了。|
|scroll|会显示滚动条。|
|hidden|溢出的元素就看不见了。|
|inherit|从父元素继承。|

#### `text-indent`

text-indent 属性规定文本块中首行文本的缩进。

#### `padding`

padding 简写属性在一个声明中设置所有内边距属性。和内边距一样，可以设置1到4个值：

```css
padding:10px 5px 15px 20px;/*上10 右5 下15 左20，也就是顺时针*/
padding:10px 5px 15px;/*上10 左右5 下15*/
padding:10px 5px;/*上下10 左右5*/
padding:10px;/*上下左右都是10*/
```

#### `border`

border 简写属性在一个声明设置所有的边框属性。

我们可以按照顺序给border设置3个值，分别是：`border-width` `border-style` `border-color`比如：
	
```css
p{
  border:5px solid red;
  }
```

这就是设置了一个5px宽的实线的红色的边框。

## 参考资料

- [MDN：position](https://developer.mozilla.org/zh-CN/docs/Web/CSS/position)：了解 CSS position 属性的基本知识
- [MDN：float](https://developer.mozilla.org/en-US/docs/Web/CSS/float)：了解 CSS float 属性的基本知识
- [Learn CSS Positioning in Ten Steps](http://www.barelyfitz.com/screencast/html-training/css/positioning/)：通过具体的例子熟悉 position 属性
- [清除浮动（clearfix hack）](http://zh.learnlayout.com/clearfix.html)：清除浮动是什么，如何简单地清除浮动
- [StackOverflow](http://stackoverflow.com/questions/211383/which-method-of-clearfix-is-best)：Which method of ‘clearfix’ is best?：清除浮动黑科技完整解读


---
layout: post
title:  "JS事件"
date:   2017-12-19
categories: web
tags: js事件
---

* content
{:toc}
## js事件

JS 是事件驱动的语言。

### 什么是事件

我们在浏览器中做的所有的操作，例如：鼠标的点击、鼠标悬停、敲击键盘等等。简单的认为事件就是用户的操作。

<!-- more -->

###什么是事件驱动

JS中会有内部机制监听这一系列的事件，当这些事件发生，JS会响应并且调用与事件相关的函数去处理该事件。

###常用的事件

| 事件名         | 描述                         |
| ----------- | -------------------------- |
| onload      | 某个页面或图像被完成加载               |
| onsubmit    | 当表单提交时触发该事件---注意事件源是表单form |
| onclick     | 鼠标点击某个对象                   |
| ondblclick  | 鼠标双击某个对象                   |
| onblur      | 元素失去焦点                     |
| onfocus     | 元素获得焦点                     |
| onchange    | 用户改变域的内容                   |
| onkeydown   | 某个键盘的键被按下                  |
| onkeypress  | 某个键盘的键被按下或按住               |
| onkeyup     | 某个键盘的键被松开                  |
| onmousedown | 某个鼠标按键被按下                  |
| onmouseup   | 某个鼠标按键被松开                  |
| onmouseover | 鼠标被移到某元素之上                 |
| onmouseout  | 鼠标从某元素移开                   |
| onmousemove | 鼠标被移动                      |

###事件绑定的方式

####  静态绑定

直接在标签上写事件对应的属性（事件名），属性对应的value，可以是一个方法

语法格式：

```html
<标签 事件名 = "函数名()" ... ></标签>

例如：

<div onclick= "show();"></div>
```

例子：

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
        <style type="text/css" >
            div{
                 background-color: #ccc;
                width: 100px;
                height: 100px;"
               }
        </style>
	</head>
	<body>
		<div  onclick="fnClick();" onmouseover="fnOver(this);" onmouseout="fnOut(this);"></div>
		<!--
			1、给div标签绑定点击事件
		2、给div标签绑定鼠标悬浮事件，使其背景颜色变红
		3、给div标签绑定鼠标移出事件，使其背景颜色变蓝-->
		<script type="text/javascript">
			function fnClick() {
				console.log('div被点击了');
			}
			function fnOver(obj) {
				obj.style.backgroundColor = 'red';
			}
			function fnOut(obj) {
				obj.style.backgroundColor = 'blue';
			}
		</script>
	</body>
</html>
```



#### 动态绑定

事件不是直接加给相应的标签，而是通过DOM技术来获取元素（标签），然后直接给标签添加事件，而事件一般后面跟着一个匿名函数

语法结构：

```js
var oDiv = document.getElementById(“div”);

oDiv.onclick = function() {

// JS逻辑代码

};
```

例子：

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<div style="background-color: #ccc; width: 100px; height: 100px;"></div>
		<!--
			1、给div标签绑定点击事件
		2、给div标签绑定鼠标悬浮事件，使其背景颜色变红
		3、给div标签绑定鼠标移出事件，使其背景颜色变蓝-->
		<script type="text/javascript">
			// 1. 获取div
			var oDiv = document.getElementsByTagName('div')[0];
			// 动态绑定一个点击事件
			oDiv.onclick = function() {
				console.log('div被点击了');
			};
			
			// 只要是函数名碰见括号就会立刻执行 不管在哪里
			oDiv.onmouseover = toRed;
			
			function toRed() {
				oDiv.style.backgroundColor = 'red';
			};
			
			oDiv.onmouseout = function() {
				this.style.backgroundColor = 'blue';
			};
		</script>
	</body>
</html>
```


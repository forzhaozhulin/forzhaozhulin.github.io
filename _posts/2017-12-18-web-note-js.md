---
layout: post
title:  "JS"
date:   2017-12-12
categories: web
tags: Javascript 定时器 
---

* content
{:toc}
# Javascript

JavaScript一种直译式脚本语言，是一种动态类型、弱类型、基于原型的语言，内置支持类型。它的解释器被称为JavaScript引擎，为浏览器的一部分，广泛用于客户端的脚本语言，最早是在HTML网页上使用，用来给HTML网页增加动态功能。

<!-- more -->

### Javascript的组成：

1. ECMAScript：JS的核心规定JS的语法和词法
2. DOM ：文档对象模型 用来和网页打交道
3. BOM： 浏览器对象模型 和浏览器打交道

### Javascript的作用：

添加动态效果

1. js可以动态的修改html及css的代码，DOM操作
2. js可以对表单进行校验

### JavaScript的引入方式

1.  内部js：也就是在html源码中嵌入js代码

```html
<script type="text/javascript">
	这里写你的js代码
</script>
```

2. 外部js：也就是将js代码单独写成一个js文件(扩展名是 .js而不是 .javascript),在html代码中引入这个封装好的js文件

```html
<script type="text/javascript" src="../xxx.js"></script>
```

一般都建议写在body标签内部最下面

因为js是阻塞时加载。他加载时别的不能执行

## Javascript的语法：

常用的语法：

```html
<script type="text/javascript">
	var 变量名 = 变量值;
	整型：  var i = 0;
	浮点型：var d = 2.35;
  	alert('信息'); //系统弹框
	console.log('信息');// 控制台
  	//tybeof
    console.log(typeof 2 );//number  
</script>
```

### 数据类型：6种

* 基本数据类型（原始数据类型）
* 数字类型：number       包含了小数和整数
* 布尔类型：boolean       只有两个值： true（真）| false（假）
* 字符串类型：string
* 未定义类型：undefined   只有一个值，即 undefined未定义
* 空类型：null             只有一个值 null，表示空

例如：

​	造一个上帝对象：  var obj = new Object();

​	造一个字符串对象：var str = new String();

​	造一个日期对象：  var date = new Date();



### 逻辑运算

在js中，不光boolean值能够参与逻辑运算。所有的值都能参与逻辑运算

* 数字0为假
* ""为假
* false为假
* null为假
* undefined  为假
* NaN(Not a Number) 为假

## 函数：

```html
function 函数名(参数列表) {
js逻辑代码
}
函数调用：函数名(实际参数);
***一定要加上小括号
```

和Java的不同点：

1. 函数需要被调用才能执行。
2. js中，如果函数需要返回值我们直接return就行了。定义函数的时候不需要声明返回值的类型，因为js是弱数据类型。
3. 在js中，如果我们需要给函数传递参数，那么我们直接写变量名就行，不需要声明传入变量的类型。
4. 在js中，不存在函数重载的概念，如果2个函数名相同，后面出现的函数会覆盖前面的函数。
5. 如果函数的声明带参数，在调用时不传参数也可以运行
6. 在js中可以用argument 来获取实际参入的参数，argument 是实参的参数数组



### 匿名函数：

不能直接调用

第一种： 将匿名函数赋值给一个变量，使用变量调用函数

定义函数并赋值给变量：

```html
var fn = function(参数列表) {
js逻辑代码
}；
调用函数：fn(实际参数);
```

第二种： 匿名函数直接作为实际参数传递

```html
function xxx( 数字类型参数，字符串类型的参数，函数类型的参数 ) {
// js逻辑代码
}
调用该函数： xxx( 100,”abc”,function(){…} );
```

### 获取元素：

```html
var oEle = document.getElementById(id的名称);
返回一个元素对象
```

解释：

​	document： 文档 我们HTML里面就是指整个网页

​	get    Element   By   Id

​	获取   一个元素   通过  Id  

​	整句翻译过来就是：在页面上通过Id来获取一个元素

​	文本框的内容是通过value这个属相来设置的，所以我们要获取文本框的内容就可以通过value这个属性来获取

### `定时器setInterval`：

window.setInterval(code, millisec) 按照指定的周期（间隔）来执行函数或代码片段。
参数1： code 必需。  执行的函数名或执行的代码字符串。 
参数2： millisec 必须。时间间隔，单位：毫秒。
返回值：一个可以传递给 window.clearInterval() 从而取消对 code 的周期性执行的值。
例如：

* 方式1：函数名 ， 	setInterval(show, 100);  
* 方式2：函数字符串，setInterval("show()" , 100);

#### 全局函数

window对象提供都是全局函数，调用函数时window.可以省略。

window.setInterval() 等效 setInterval()

window.onload = function() {
​	// 页面加载完成的时候会执行这个函数
};

1.  为页面设置加载事件onload
2.  给轮播图的图片设置一个id
3.  根据id来获取到轮播图的图片
4.  开启定时器，2000毫秒重新设置图片的src属性

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>

		<script type="text/javascript">
			// 开启定时器隔一秒来输出一次 ‘hello world’
			// setInterval(code, millisec)
			//  code : 函数名或者匿名函数  millisec： 时间间隔  毫秒为单位
			var timer = window.setInterval(function(){
				console.log('hello world');
			},1000);

			// 关闭定时器
			// clearInterval(定时器的名字：返回值);
			window.clearInterval(timer);
		</script>

	</body>
</html>
```

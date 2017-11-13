---
layout: post
title:  "JavaScript函数"
date:   2017-03-15 12:45:54
categories: 前端
tags: JavaScript
author: 薛彬
---

* content
{:toc}





## 概念

> ECMAScript中的函数使用`function`关键字来声明，后跟一组参数以及函数体。

在JavaScript中，每个函数其实都是一个Function对象。

```javascript
function functionName(arg0,arg1...argN){
    statements
}
```

如果一个函数中没有使用return语句，则它默认返回undefined，可以通过`return`来实现返回值，例如：

```javascript
function sum(num1,num2){
    return num1+num2;
}
var sumResult=sum(1,2);
console.log(sumResult);//3
```

函数会在执行完`return`语句之后停止并立即退出，所以`return`语句之后的代码永远不会执行，不过有个例外，等等会讲到。

## 函数声明、函数表达式

### 函数声明

如下就是函数声明：

> function name([param[, param[, ... param]]]) { statements }

- name:函数名
- param：参数
- statements:函数体

### 函数表达式

> var name=function([param] [, param] [..., param]){ statements }

- name:函数名
- param：参数
- statements:函数体

### 声明提升

> 解析器在向执行环境中加载数据时，对函数声明和函数表达式并非一视同仁。解析器会率先读取函数声明，并使其在执行任何代码之前可用，至于函数表达式，则必须等到解析器执行到它所在的代码行才能正在被解释执行。

也就是说，即使在`function name(){}`声明前使用`name()`也是可以的，但是在`var name=function(){}`就是不行的。

这就叫做函数声明提升（functiondeclaration hoisting）。

看看这个例子：

```javascript
getName();//axuebin
function getName(){
    var name="axuebin";	
    console.log(name);
}
sum(1,2);//sum is not a function
var sum=function(num1,num2){
    return num1+num2;
}
```

第一段代码中`getName()`完全可以正常运行，因为在代码执行之前，解析器就已经读取并将函数声明添加到执行环境中。

第二段代码就会产生错误。

## IIFE

IIFE即Immediately-Invoked Function Expression，立即执行函数表达式，代码如下：

```javascript
(function(){return 1}())//1
```

使用立即执行的函数表达式有两个目的：

- 不必为函数命名，避免污染全局变量。
- 内部形成一个单独的作用域，可以封装一些外部无法读取的私有变量。

## 参数

JavaScript中函数的参数有实参和形参的区别，函数不介意传递进来多少个参数，也不在乎传进来的参数是什么数据类型。

实际上，函数内部接受到的是一个数组，用`arguments`表示，所以无论传递多少个参数，即使没有传递参数，接收到的也就只是`[]`而已，不会报错。

我们可以通过访问`arguments`数组来获取传递给函数的每一个参数。

```javascript
function getInfo(name,age){
    console.log(name+":"+age+"岁");
    console.log(arguments[0]+":"+arguments[1]+"岁"+","+arguments[2]);
}
getInfo("axuebin","25","student");
```

这里我们定义了两个形参`name`和`age`，在调用`getInfo`时传入了第三个参数也可以执行，而且内部通过`arguments`可以获取的到，此时形参为2个，但是实参为3个。

### ES6的rest参数

ES6引入了rest参数，形式为`...变量名`，用于获取函数的多余参数，这样就可以不用`arguments`对象了。

```javascript
function sum(...numbers){
    let sum=0;
    for(var val of values){
		sum+=val;
    }
    return sum;
}
sum(1,2,3);//6
sum(1,2,3,4);//10
```


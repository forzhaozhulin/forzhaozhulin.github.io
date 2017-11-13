---
layout: post
title:  "JavaScript作用域"
date:   2017-03-14 16:14:54
categories: 前端
tags: JavaScript
author: 薛彬
---

* content
{:toc}


作用域就是所有变量和函数的可访问范围，JavaScript只有全局作用域和局部作用域两种，没有块级作用域。





## 作用域

全局作用域就是代码的任何地方都可以访问到，而局部作用域就是只有在定义变量的那个执行局部环境中才可使用。

```javascript
var globalName="axuebin";
console.log(globalName);//axuebin
function getName(){
    console.log(globalName);//axuebin
    var name="xb";
    age=25;
    console.log(name);//xb
    console.log(age);//25
}
getName();
console.log(name);//undefined
console.log(age);//25
```

- `globalName`在`getName`函数外定义的，在全局作用域中，任何地方都可以访问。
- `name`是在`getName`函数内定义的，属于局部变量，外部无法访问，在函数退出后就会被销毁。
- `age`是在`getName`函数内赋值的，但是没有使用`var`来定义。这种未定义就赋值的变量自动声明成全局变量。

### 没有块级作用域

JavaScript没有块级作用域。

```javascript
if(true){
    var name="axuebin";
}
console.log(axuebin);
```

上诉的代码若在Java等语言中，在if语句执行完之后`name`变量就会被销毁，而在JavaScript中，if语句会将变量添加到当前的执行环境中（例子中是全局环境）。

## 作用域链

当代码在一个环境中执行时，会创建变量对象的一个作用域链（scope chain）。

> 作用域链的用途是保证对执行环境有权访问的所有变量和函数的有序访问。

当一个变量被使用时，会先在当前环境中搜索，若当前环境中未找着定义的变量，则会沿着作用域链逐级向后搜索，直到找着为止，全局执行环境是作用域链中最后一个对象。

```javascript
var firstName="axuebin";
function getSecondName(){
    var secondName="xuebin";
    function getThirdName(){
        var thirdName="xb";	
        console.log(firstName);	
    } 
    getThirdName();
}
getSecondName();
```

比如此时在`getThirdName`中控制台输出`firstName`,会先在当前函数的环境中查找该变量，若找不到就在外层也就是`getSecondName`中查找，直至在最外层找着`var firstName`。

在这里值得一提的是，若在函数内部要多次使用一个全局变量，每次在作用域链会影响效率，此时可以在函数内部将全局变量先存在一个局部变量后再使用。


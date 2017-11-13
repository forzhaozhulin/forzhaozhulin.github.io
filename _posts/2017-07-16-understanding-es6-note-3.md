---
layout: post
title:  "小白学《深入理解es6》--函数"
date:   2017-07-17 13:17:54
categories: 前端
tags: ES6
author: 薛彬
---

* content
{:toc}





## 函数形参的返回值

ECMAScript函数不介意传递来几个参数，即使定义了一个参数，传进来10个参数都是可以的。这是因为参数在函数内部是用一个数组来表示的，也就是说函数接收到的始终是这个数组，即使数组元素为空。

在函数体内，可以通过`arguments`对象来访问这个参数数组，从而获得每一个参数。

函数参数可以分为形参和实参，在JS中，我们可以这样获取两个参数的长度：

```javascript
function foo(a,b){
  var formalParamLen=arguments.callee.length;
  var actualParamLen=arguments.length;
  console.log(formalParamLen);
  console.log(actualParamLen);
};
foo() // 2,0
foo(1) // 2,1
foo(1,2) // 2,2
foo(1,2,3) // 2,3
```

通过执行上面的代码可以发现，`arguments`是实参，在函数内部就可以通过`arguments[0]`等来访问。

但是 ~ `arguments`其实不是一个数组，`Array.isArray(arguments)`的结果是`false`,`typeof arguments`的结果是`object`。

好了，来看看和ES6相关的内容吧~

### 在ECMAScript5中模拟默认参数

通过上文我们知道即使定义了参数也是可以不传递的，所以严谨的做法是是为定义的参数赋予一个默认值。

在ECMAScript5中，往往这样来给参数赋予默认值：

```javascript
function getPrice(name,type){
  type=(typeof type !== "undefined") ? type : "All";
}
```

### ECMAScript6中的默认参数值

在ES6中，就简单了~

```javascript
function getPrice(name,type="All"){
  console.log(name+":"+type);
  console.log(arguments.length)
}
getPrice("apple"); // apple:All
getPrice("apple","China"); // apple:China
```

效果是一样哒~


### 默认参数值对arguments对象的影响

在在ECMAScript5中，比如通过`var param1 = arguments[0]`获取了第一个参数之后对这个`param1`进行修改，则连着`arguments[0]`也会被修改，这是不安全的。

```javascript
function getPrice(name){
  console.log(name); // ES5
  name="ES6";
  console.log(name); //ES6
  console.log(arguments[0]); ES6
}
```

在ES6中，通过赋予默认参数，这种情况就不会发生了：

```javascript
function getPrice(name="ES5"){
  console.log(name); // ES5
  name="ES6";
  console.log(name); //ES6
  console.log(arguments[0]); //undefined
}
```

## 处理无命名参数

### 不定参数

之前说过，可以通过`arguments`来检查函数的实参，在ES6中，通过引入不定参数(rest parmaeters)的特性来解决这个问题。

在函数的参数命名前加三个点（`...`）就表明这是一个不定参数，该参数是一个数组。与`arguments`不同的是，不定参数不包括`...`之前传入的参数。

- 不定参数之后不能有其他参数
- 不能用于对象字面量的setter中

```javascript
function foo(a,...array){
  console.log(arguments.length); // 4
  console.log(array.length); // 3
  console.log(arguments[0]); // 1
  console.log(array[0]); // 2
}
foo(1,2,3,4);
```

不管有没有使用不定参数，`arguments`都是包含所有传入的参数。

## 增强的Function构造函数

允许在`Function`构造函数中使用不定参数：

```javascript
var foo=new Function("...args","return args");
foo(1,2,3) //[1,2,3]
```

## name属性

`name`属性用来获取函数的名称。

```javascript
function getJob(){
  console.log("Give me a job!")
}
console.log(getJob.name) // getJob
```

如果是这样的呢？

```javascript
var getJob=function giveMeJob(){
  console.log("Give me a job!")
}
console.log(getJob.name) // giveMeJob
```

发现输出的是giveMeJob，这是因为函数表达式赋予了一个名字，这个名字比本身被赋值的变量的权重高。

特殊情况：

- 通过`bind()`函数创建的函数
- 通过`Function`构造函数创建的函数

```javascript
var foo(){};
console.log(foo.bind().name); //bound foo
console.log((new Function()).name); //anonymous
```

## 明确函数的多重用途

### 块级函数

ECMAScript5的严格模式中，当在代码块内部声明函数的时候程序会抛出错误：

```javascript
"use strict";
if(true){
  function doSomething(){
    //todo
  }
}
//这样会报错
```

ECMAScript 6 中，会将`doSomething()`函数视作一个块级声明，从而可以在定义该函数的代码块内访问和调用它。

```javascript
"use strict";
//ES6
if(true){
  console.log(typeof doSomething); //function
  function doSomething(){
    //todo
  }
  doSomething() //undefined
}
```

如果需要函数提升至代码顶部，则选择块级函数；否则，用let表达式来声明函数。

## 箭头函数

- 没有`this``super``arguments``new.target`绑定
- 不能通过`new`关键字调用
- 没有原型
- 不可以改变`this`的绑定
- 不支持`arguments`对象
- 不支持重复的命名参数

没有参数时：

```javascript
（）=>"hello world"
//相当于
function(){
  return "hello world";
}
```

一个参数时：

```javascript
value=>value
//相当于
function(value){
  return value;
}
```

函数体内多条语句：

```javascript
（value1，value2）=>{return value1+value2}
//相当于
function(value1，value2){
  return value1+value2;
}
```

返回对象：

```javascript
（id,name）=>({id:id,name:name})
（id,name）=>({id,name}) //ES6中可以缩写
//相当于
function(id，name){
  return {
    id:id,
    name:name
  }
}
```

立即执行函数表达式：

```javascript
((name)=>{
  return {
    getName:()=>name
  }
})("axuebin")
```

## 尾调用优化

> 尾调用指的是函数作为另一个函数的最后一条语句被调用。

```javascript
function doSomething(){
  return doSomethingElse(); // 尾调用
}
```

在ES5中，尾调用的实现其实是：创建一个新的栈帧，将其推入调用栈来表示函数调用，在循环调用的中，每一个未用完的栈帧都会被保存在内中，这是不好的~

### ES6中的优化

缩减了尾调用栈的大小，如果满足以下条件，就清楚并重用当前调用栈帧：

- 尾调用不访问当前栈帧的变量
- 函数内部，尾调用是最后一条语句
- 尾调用的结果当做函数值返回

就是说，要确保返回的函数调用后不执行其他操作。



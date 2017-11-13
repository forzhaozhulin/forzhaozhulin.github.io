---
layout: post
title:  "小白学《深入理解es6》--Promise与异步编程"
date:   2017-07-25 15:42:10
categories: 前端
tags: ES6
author: 薛彬
---

* content
{:toc}





`Promise`既可以像事件和回调函数一样指定稍后执行的代码，也可以明确指示代码是否成功执行。

为了让代码可读性更强，也更容易调试，可以链式地编写`Promise`。

## 异步编程的背景知识

JavaScript引擎是基于单线程（Single-threaded）事件循环的概念构建的，同一个时刻只允许一个代码块在执行。

因此，即将运行的代码被放在一个任务队列（job queue）中。每当JavaScript引擎中的一段代码结束执行，事件循环（event loop）会执行队列中的下一个任务。

`event loop`是JavaScript引擎中的一段程序，负责监控代码执行并管理任务队列。

## Promise的基础知识

### Promise的生命周期

- pending：进行中，操作尚未完成，未处理（unsettled）
- settled：已处理，异步操作执行结束
- fulfilled：异步操作成功完成
- rejected：出错，异步操作未能完成

内部属性`[[PromiseState]]`用来表示三种状态：`pending`,'fulfilled','refjected'。这个属性不对外暴露，所以只含有通过`then()`来根据不同状态执行不同的操作。

`then()`方法接受两个参数：成功时的函数，失败时的函数。

```javascript
promise.then(function(a){
  //完成
},function(err){
  //拒绝
})
```

如果只要成功的，后面一个参数就直接省略；而如果只要失败的，就要给第一个参数的位置赋个`null`。

### 创建未完成的Promise

用`Promise`构造函数可以创建新的`Promise`，构造函数只接受一个参数：包含初始化Promise代码的执行器函数。

执行器接受两个参数：`resolve()`和`reject()`。

```javascript
function a(){
  return new Promise(function(resolve,reject){
    //...  
  })
}
let promise=a();
a.then(function(){
  //完成
},function(err){
  //拒绝
})
```

## 串联Promise

每次调用`then()`方法或`catch()`方法时实际上创建并返回了另一个`Promise`，这就可以用来将多个`Promise`串联起来实现复杂的异步特性：

```javascript
let p1=new Promise(function(resolve,reject){
  resolve(1);
})
p1.then(function(value){
  console.log(value);
}).then(function(){
  console.log(3);
})
```

当执行玩`p1.then()`之后返回了一个Promise,可以继续调用它的`then()`方法，只有当第一个Promise被解决之后才会调用第二个Promise的`then()`方法。

### 捕获错误

Promise链可以用来捕获处理程序中可能发生的错误：

```javascript
let p1=new Promise(function(resolve,reject){
  resolve(1);
})
p1.then(function(value){
  throw new Error("boom!");
}).catch(function(error){
  console.log(error.message); // boom!
})
```

在Promise链的末尾留有一个拒绝处理程序可以确保能够正确处理所有可能发生的错误。

### Promise链的返回值

Promise链的另一个重要特性是可以给之后的Promise传递数据。

## 响应多个Promise

可以使用`Promise.all()`和`Promise.race()`两个方法来监听多个Promise。

### Promise.all()

接受一个参数并返回一个Promise，该参数是一个含有多个受监视Promise的可迭代对象。

只有可迭代对象中所有的Promise都被完成后才算是整个Promise被完成。

```javascript
let p1=new Promise();
let p2=new Promise();
let p3=new Promise();
let p4=Promise.all([p1,p2,p3]);
p4.then();
```

### Promise.race()

`race`和`all`的区别是，`race`接受的多个Promise中只要有一个Promise被完成，整个返回的Promise就可以完成。

## 基于Promise的异步任务执行

> 未完待续~

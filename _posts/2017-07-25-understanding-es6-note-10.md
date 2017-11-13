---
layout: post
title:  "小白学《深入理解es6》--改进的数组功能"
date:   2017-07-25 11:00:10
categories: 前端
tags: ES6
author: 薛彬
---

* content
{:toc}





## 创建数组

创建数组的方式之前有两种：

- 调用Array构造函数
- 数组字面量

## Array.of()

`Array`构造函数有时候的结果和预料的不太一样：

```javascript
let items=new Array(2); // [undefined,undefined]
let items=new Array("2"); // ["2"]
let items=new Array(1,2); // [1,2]
let items=new Array(3,"2"); // [3,"2"]
```

给`Array`构造函数传入不同的值，数组的结果就不同。

- 如果只传入一个数字类型，则会当成是数组长度
- 如果传入多个值，则会当成数组元素

这样的特性会让人困惑。

ES6引入`Array.of()`来解决这个问题，无论是有多少个参数，无论参数是什么类型，`Array.of()`方法总会创建一个包含所有参数的数组。

```javascript
let items=Array.of(2); // [2]
let items=Array.of("2"); // ["2"]
let items=Array.of(1,2); // [1,2]
let items=Array.of(3,"2"); // [3,"2"]
```

### Array.from()

`Array.from()`方法可以接受可迭代对象或者类数组对象作为第一个参数，最终返回一个数组。

`Array.from()`方法还可以传入第二个参数，可以是一个回调函数，表示对数组的进一步操作。

#### 用Array.from()转换可迭代对象

`Array.from()`方法可以处理类数组对象和可迭代对象，也就是能将所有含有`Symbol.iterator`属性的对象转换为数组。

## 为所有数组添加的新方法

### find()、findIndex()

`find()`和`findIndex()`都接受两个参数：回调函数、可选参数（指定回调函数中this的值）。

执行回调函数时，传入的参数分别是：

- 数组中的某个元素
- 该元素在数组中的索引
- 数组本身

如果找到了立即返回结果并停止搜索。

`find()`返回查找到的值，`findIndex()`返回查找到值的索引。

### fill()

`fill()`方法可以用指定的值填充一至多个数组元素。

`fill()`有两个参数：填充的值、开始索引值、结束索引值。

如果只传入一个参数，则会用这个值重写数组：

```javascript
let arr=[1,2,3,4];
arr.fill(1);
console.log(arr); // [1,1,1,1]
```

如果传入两个参数，则会把`arr.length`当做结束索引：

```javascript
var arr=[1,2,3,4];
arr.fill(1,2);
console.log(arr); // [1,2,1,1]
```

如果传入三个参数，则开始于第二个参数，结束于第三个参数：

```javascript
var arr=[1,2,3,4];
arr.fill(1,1,3);
console.log(arr); // [1,1,1,1]
```

### copyWithin()

从数组中赋值元素的值。调用`copyWith()`方法时传入两个参数：开始填充的索引位置，开始复制值的索引位置，比如：

```javascript
let arr=[1,2,3,4];
arr.copyWithin(2,0);
console.log(arr); // [1,2,1,2]
```

默认情况下，会一直复制直到数组末尾的值。

第三个参数：停止复制的索引位置。

如果引入第三个参数，就会手动控制停止复制，比如：

```javascript
let arr=[1,2,3,4];
arr.copyWithin(2,0,1);
console.log(arr); // [1,2,1,4]
```

## 定性数组

> 定性数组是一种用于处理数值类型数据的专用数组。

### 数值数据类型

JavaScript是用64个比特来存储一个浮点形式的数据。

定型数组支持存储和操作以下8种不同的数值类型：

- int8
- uint8
- int16
- uint16
- int32
- uint32
- float32
- float64

如果用普通的数字来存储8位整数，就会浪费56个比特，这也就是定型数组的一个用例，更有效的地利用比特。

在使用定型数组之前，需要创建一个数组缓冲区存储这些数据。

### 数组缓冲区

数组缓冲区是所有定型数组的根基，它是一段可以包含特定数量字节的内存地址。可以通过`ArrayBuffer`构造函数来创建数组缓冲区：

```javascript
let buffer=new ArrayBuffer(10); //分配10个字节
```

创建后可以通过`byteLength`属性来查看缓冲区中的比特数量：

```javascript
console.log(buffer.byteLength); //10
```

### 通过视图操作数组缓冲区


这部分之后先不看了...
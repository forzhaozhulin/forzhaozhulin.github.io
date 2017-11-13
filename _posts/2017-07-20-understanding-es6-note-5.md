---
layout: post
title:  "小白学《深入理解es6》--解构"
date:   2017-07-20 10:43:10
categories: 前端
tags: ES6
author: 薛彬
---

* content
{:toc}





## 如何使用解构功能

### 对象解构

利用对象字面量的语法来实现解构：

```javascript
let person={
  name:"axuebin",
  age:25
}
let {name,age}=person
console.log(name) // axuebin
console.log(age) // 25
```

通过上述方式就可以直接从`person`对象中读取相应值。

注意：使用`var`、`let`、`const`来解构声明变量，必须初始化。

### 解构赋值

```javascript
let person={
  name:"axuebin",
  age:25
}
let name="a";
let age=1;
({name,age}=person)
console.log(name) // axuebin
console.log(age) // 25
```

赋值的时候必须要用一对`()`将结构赋值语句包裹起来。

### 默认值

使用解构赋值表达式时，如果指定的变量名称在对象中不存在，就会被赋值成`undefined`，或者像函数的默认值一样可以直接赋值，当不存在的时候就使用默认值。

### 为非同名局部变量赋值

使用不同命名的局部变量来存储对象属性的值：

```javascript
let person={
  name:"axuebin",
  age:25
}
let {name:localName,age:localAge}=person;
console.log(localName) // axuebin
console.log(localAge) // 25
```

`name:localName`的含义是：读取名为`name`的属性并将其值存储在变量`localName`中。

## 数组解构

使用数组字面量来解构，且解构操作全部在数组内完成。

在数组解构的语法中，通过值在数组中的位置进行选取，可以直接省略元素。

```javascript
let colors=["red","green","blue"];
let [,,thirdColor]=colors;
console.log(thirdColor);// blue
```

### 解构赋值

数组的解构赋值不需要用`()`包起来，就直接用`[]`即可。

```javascript
let colors=["red","green","blue"];
let firstColor="white";
let lastColor="black";
[firstColor,lastColor]=colors;
console.log(firstColor); // red
console.log(lastColor); // green
```

利用数组的解构赋值，我们可以很轻松的交换数组中的值。

```javascript
let [a,b]=[1,2];
console.log(a); // 1
[a,b]=[b,a];
console.log(a); // 2
```

### 默认值

可以在数组解构赋值表达式中为数组的任意位置添加默认值，当指定位置的值不存在或者为`undefined`时使用默认值。

```javascript
let name=["xue"];
let [firstName,secondName="bin"]=name;
console.log(firstName); // xue
console.log(secondName); // bin
```

### 不定元素

在数组中可以使用`...`将数组中的其余元素赋值给一个变量：

```javascript
let colors=["red","green","blue"];
let [firstColor,...restColors]=colors;
console.log(firstColor); // red
console.log(restColors); // ["green","blue"]
```

这个可以用来对数组进行复制：

```javascript
let colors=["red","green","blue"];
let [...colorsClone]=colors;
console.log(colorsClone); // ["red","green","blue"]
```

注意：不定元素必须为最后一个条目。


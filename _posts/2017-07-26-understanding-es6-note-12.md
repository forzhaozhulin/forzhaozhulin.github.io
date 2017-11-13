---
layout: post
title:  "小白学《深入理解es6》--用模块封装代码"
date:   2017-07-26 9:56:10
categories: 前端
tags: ES6
author: 薛彬
---

* content
{:toc}





## 什么是模块

模块：

- 模块是自动运行在严格模式下并没有办法退出运行的JavaScript代码。

- 模块顶部创建的变量尽在模块的顶级作用于中存在，不是全局共享。

- 模块必须导出一些外部代码可以访问的元素。

脚本：

- 任何不是模块的JavaScript代码。

模块最重要的就是仅导入和导出需要的元素，而不是将所有东西都放到一个文件中。

## 导出的基本语法

可以用`export`关键字将一部分已发布的代码暴露给其他模块。

```javascript
export var name="axuebin";
export let age=25;
export function findJob(){
  console.log("FE");
}
```

## 导入的基本语法

从模块中导出的元素可以通过`import`关键字在另一个模块中访问。

```javascript
import {identifier1,identifier2} from "example.js";
```

大括号表示从给定模块导入的绑定，关键字`from`表示从哪个模块导入给定的绑定。

### 导入单个绑定

```javascript
import {findJob} from "./aboutMe.js";
findJob(); // FE
```

### 导入多个绑定

```javascript
import {name,age} from "./abountMe.js";
console.log(name); // axuebin
console.log(age); // 25
```

### 导入整个模块

```javascript
import * as abountMe from "./abountMe.js";
console.log(abountMe.name); // axuebin
console.log(abountMe.age); // 25
```

注意：只能在代码顶部使用`import`和`export`。

## 导出和导入时重命名

- 要使用不同的名称导出一个函数，则使用`as`关键字指定一个名称

```javascript
function sum(num1,num2){
  return num1+num2;
}
export {sum as add};
```

在导入这个模块时，就必须使用`add`：

```javascript
import {add} from "./example.js";
```

- 导入时也一样

```javascript
import {sum as add} from "./example.js";
console.log(add(1,2)); // 3
```

## 模块的默认值

模块的默认值是通过`default`关键字来指定的单个变量、函数或者类，只能为每个模块设置一个默认的导出值。

### 导出的默认值

```javascript
//1
export default function(num1,num2){
  return num1+num2;
}
//2
function sum(num1,num2){
  return num1+num2;
}
export default sum;
```

### 导入默认值

```javascript
import sum from "./example.js";
console.log(sum(1,2)); // 3
```

这里在导入的时候并没有使用`{}`包裹起来，这用于表示模块导出的任何默认函数。

我们还可以同时导入默认值和非默认值：

```javascript
//导出
export let name="axuebin";
export default function(){
  console.log("FE");
}

//导入
import findJob,{name} from "./aboutMe.js";
console.log(findJob()); // FE
console.log(name); // axuebin
```

注意：默认值必须在非默认值前。




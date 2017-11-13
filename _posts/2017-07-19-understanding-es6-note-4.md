---
layout: post
title:  "小白学《深入理解es6》--对象"
date:   2017-07-19 17:53:10
categories: 前端
tags: ES6
author: 薛彬
---

* content
{:toc}





## 对象类别

- 普通对象：具有JavaScript对象所有的默认内部行为
- 特异对象：具有某些与默认行为不符的内部行为
- 标准对象
- 内建对象：脚本开始执行时存在于JavaScript执行环境中的对象，所有标准对象都是内建对象。

## 对象字面量语法扩展

### 属性初始值的简写

当一个对象的属性与本地变量同名时，不必要再写冒号和值，只要写属性名就好。

```javascript
function createPerson(name,age){
  return{
    name,
    age   
  }
}
```

JavaScript引擎会在可访问作用域中查找其同名变量。

### 对象方法的简写语法

定义对象的时候，为对象添加方法可以省略冒号和`function`关键字：

```javascript
var person={
  name:"axuebin",
  sayName:function(){
    console.log(this.name)
  }
}
// ES6
var person={
  name:"axuebin",
  sayName(){
    console.log(this.name)
  }
}
```

### 可计算属性名

之前，我们如果要通过变量来获取对象的属性，就必须要用`[属性名]`的形式。

但是在定义对象的时候，属性名必须是已知的。

ES6中，可在对象字面量中使用可计算属性名称，语法也是`[属性名]`的形式：

```javascript
let lastName="lastName";
let person={
  "firstName":"xue",
  [lastName]:"bin"
}
console.log(person);
```

## 新增方法

### Object.js()方法

弥补全等运算符的不准确运算，接受两个参数，如果两个参数**类型相同**且具有**相同的值**，则返回true。


```javascript
console.log(+0===-0); //true
console.log(Object.is(+0,-0)); //false
console.log(NaN===NaN); //false
console.log(Object.is(NaN,NaN)); //true
```

### Object.assign()方法

这个方法可以接受任意数量的源对象，并按指定的顺序将属性复制到接受对象中，如果同名，后面的会覆盖前面的。

## 重复的对面字面量属性

如果对象声明了重复的属性名，不会报错，会选取最后的一个值。

## 自有属性枚举顺序

 ES6规定了对象自有属性被枚举时的返回顺序，规则如下：

- 所有数字键按升序排序
- 所有字符串键按照它们被加入对象的顺序排序
- 所有symbol键按照它们被加入对象的顺序排序

## 增强对象原型

### 改变对象的原型

通过构造函数或者`Object.create()`方法创建对象时就指定了原型，对象原型在实例化后就保持不变。

`Object.getPrototypeOf()`返回任意指定对象的原型。

ES6中：

`Object.setPrototypeOf()`改变任意指定对象的原型，接受两个参数：被改变原型的对象、替代第一个参数原型的对象。

### 简化原型访问的Super引用

Super引用相当于指向对象原型的指针，实际上就是`Object.getPrototypeOf(this)`的值，其实就是调用原型的同名方法。

`super`必须要使用简写方法的对象中使用。

用`super`调用原型上的方法，此时的`this`绑定会自动设置成当前作用域的`this`。
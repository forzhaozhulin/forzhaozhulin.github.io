---
layout: post
title:  "小白学《深入理解es6》--迭代器和生成器"
date:   2017-07-23 15:10:10
categories: 前端
tags: ES6
author: 薛彬
---

* content
{:toc}





Set集合是一种无重复的列表，通常的做法是检测给定的值在某个集合中是否存在。

Map集合内含多组键值对，集合中每个元素分别存放着可访问的键名和它对应的值，常被用于缓存频繁取用的数据。

## ES6中的Set集合

ES6新增的Set类型是一种有序列表，其中含有一些相互独立的非重复值，通过Set集合可以快速访问其中的数据。

### 创建Set集合并添加元素

调用`new Set()`创建Set集合，调用`add()`方法向集合中添加元素，通过`size`获取元素数量。

```javascript
let set =new Set();
set.add(5);
console.log(set); // {5}
set.add(5);
console.log(set); // {5}
set.add(6);
console.log(set); // {5,6}
console.log(set.size); // 2
```

可以通过数组来初始化Set集合，过滤其中重复的值，这就为数组的去重提供了一个方法。

```javascript
//方法一
function unique(array){
  return Array.from(new Set(array))
}
//方法二
function unique(array){
  return [...new Set(array)]
}
```

通过`has()`方法可以检测Set集合中是否存在某个值

### 移除元素

调用`delete()`方法可以移除Set集合中的某一个元素，调用`clear()`方法会移除集合中的所有元素。

### Set集合的forEach()方法

`forEach`的回调函数接受以下3个参数：

- Set集合中下一次索引的位置
- 与第一个参数一样的值
- 被遍历的Set集合本身

### 将Set集合转换为数组

用展开运算符`...`可以将数组中的元素分解为各自独立的函数参数。

```javascript
let set=new Set([1,1,2,3,4,5]);
let array=[...set]
console.log(array); // [1, 2, 3, 4, 5]
```

### Weak Set集合

在Set中，如果存入一个对象，即使这个对象被回收了，Set中还保持着对这个对象的引用，如果希望对象回收后，Set中的引用也被清除，就需要用到`Weak Set`集合（弱引用Set集合）。

Weak Set集合直接利用构造函数创建，支持`add()`,`has()`,`delete()`三个方法。

```javascript
let set =new WeakSet();
let key={};
set.add(key);
console.log(set.has(key)); // true
set.delete(key);
console.log(set.has(key)); // false
set.add(key);
key=null;
console.log(set.has(key)); // false
```

## ES6中的Map集合

ES6中，Map类型是一种储存着许多键值对的有序列表，其中的键名和对应的值支持所有的数据类型。

通过`set(key,value)`方法往Map集合中添加新的元素，通过`get(key)`获取元素，如果key不存在，则返回`undefined`。

### Map集合支持的方法

- `has()`：检测键名在Map集合中是否存在
- `delete()`：移除指定键名和值
- `clear()`：清空Map集合
- `size`：包含键值对数量

### Map集合的Foreach方法

`forEach`的回调函数接受以下3个参数：

- Map集合中下一次索引的位置
- 值对应的键名
- 被遍历的Map集合本身

### Weak Map集合

> 未完待续~
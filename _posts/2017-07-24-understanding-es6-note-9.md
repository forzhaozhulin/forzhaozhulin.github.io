---
layout: post
title:  "小白学《深入理解es6》--JavaScript中的类"
date:   2017-07-24 14:13:10
categories: 前端
tags: ES6
author: 薛彬
---

* content
{:toc}





## ECMAScript5中的近类结构

在ES5中是利用原型来实现近类结构，先是创建一个构造函数，然后定义另一个方法并赋值给构造函数的原型。

```javascript
function Person(name){
  this.name=name;
}
Person.prototype.sayName=function(){
  console.log(this.name);
}
var person1=new Person("axuebin");
person1.sayName(); // axuebin
console.log(person1 instanceof Person); //true
console.log(person1 instanceof Object); //true
```

## 类的声明

### 基本的类声明语法

使用`class`关键字，紧跟着类的名字，语法类似对象字面量的简写模式，各个属性间不用使用`,`分隔。

比如，上文的写法可以用ES6改成这样：

```javascript
class Person{
  constructor(name){
    this.name=name;
  }
  sayName(){
    console.log(this.name);
  }
}
let person1=new Person("axuebin");
person1.sayName(); // axuebin
console.log(person1 instanceof Person);
console.log(person1 instanceof Object);
```

私有属性是实例中的属性，不会出现在原型上，且只能在类的构造函数或方法中创建，这里的`name`就是私有属性。

### 为何使用类语法

类与自定义类型之间的差异：

- 类声明不能被提升
- 类声明中的所有代码将自动运行在严格模式下
- 类中的所有方法都是不可枚举的
- 使用`new`之外的方式调用类的构造函数会导致程序抛出错误
- 在类中修改类名会导致程序报错

## 类表达式

### 基本的类表达式语法

```javascript
let Person=class{
  construtor(name){
    this.name=name;
  }
  sayName(){
    console.log(this.name);
  }
};
let person1=new Person("axuebin");
```

### 命名类表达式

```javascript
let Person=class Person2{
  construtor(name){
    this.name=name;
  }
  sayName(){
    console.log(this.name);
  }
};
let person1=new Person("axuebin");
```

在这之中类表达式被命名为`Person2`,但是`Person2`是存在于类定义中，相当于内部定义了一个类，然后return了。

## 作为一等公民的类

> 一等公民：一个可以传入函数，可以从函数返回，并且可以赋值给变量的值。

JavaScript中函数是一等公民。

ES6中，将类也设计成一等公民。

## 可计算成员名称

类方法和访问其属性支持使用可计算名称，就是用方括号包裹一个表达式即可使用可计算名称：

```javascript
let methodName="sayName";
class Person{
  construtor(name){
    this.name=name;
  }
  [methodName](){
    console.log(this.name);
  }
};
let person1=new Person("axuebin");
person1.sayName(); // axuebin
```

## 生成器方法

在类中，也可以通过在方法名前附加一个星号(`*`)来定义生成器。

```javascript
class Person{
  *createIterator(){
    yield 1;
    yield 2;
    yield 3;
  }
};
let person1=new Person();
let iterator=person1.createIterator(); 
```

如果类是表示值的集合的，可以为它定义一个默认迭代器，通过`Symbol.iterator`定义生成器方法即可：

```javascript
class Collection(){
  constructor(){
    this.items=[];
  }
  *[Symbol.iterator](){
    yield *this.items.values();
  }
}
let collection=new Collection();
collection.items.push(1);
collection.items.push(2);
collection.items.push(3);
for(let x of collection){
  console.log(x); // 1,2,3
}
```

## 静态成员

成员名前加`static`关键字，类中所有方法和访问器属性都可以定义。

注意：不可在实例中访问静态成员，必须在类中访问。

## 继承与派生类

ES6中可以用`extends`实现类的继承：

```javascript
class Rectangle{
  constructor(length,width){
    this.length=length;
    this.width=width;
  }
  getArea(){
    return this.length*this.width;  
  }
}
class Square extends Rectangle{
  constructor(length){
    super(length,length);
  }
}
var square=new Square(3);
console.log(square.getArea()); // 9
console.log(square instanceof Square); // true
console.log(square instanceof Rectangle); // true
```

通过`extends`关键字实现继承，并在构造函数中通过`super()`调用父类构造函数并传入相应参数。

如果在派生类中指定了构造函数，则必须要调用`super()`，如果不适用构造函数则会自动调用。

### 类方法遮蔽

派生类中的方法总会覆盖基类中的同名方法，但可以通过`super.方法名()`来调用父类的方法。

### 静态成员继承

如果基类有静态成员，那么这些静态成员在派生类中也可使用。

### 内建对象的继承

### Symbol.species属性

## 在类的构造函数中使用new.target


---
layout: post
title:  "《JavaScript语言精粹》笔记一"
date:   2016-07-25 09:03:00
categories: 前端
tags: JavaScript
author: 薛彬
---

* content
{:toc}

记录一下阅读《JavaScript语言精粹》的笔记。





## 语法

### 注释

- 注释一定要精确的描述代码，宁缺毋滥。

    - /* */包围的块注释
    - //开头的行注释

- 对于第一种写法，`/**/`符号有可能出现在正则表达式中，这是不安全的，所以建议用第二种写法来写注释。

### 数字

- 可以使用指数。

```javascript
100===1e2 // true
```

- `NaN`是一个数值，它表示一个不能产生正确结果的运算结果。可以通过`isNaN(number)`来检测`NaN`。

### 字符串

- 可以包在一对单引号或双引号中，可能包含0或多个字符。

- `\`是转义字符。

- 字符串是不可变的，一旦被创建就无法改变，但可以通过`+`来连接字符串。

### 语句

- 每一个`<script></script>`标签提供一个被编译且立即执行的编译单元，每一个编译单元都包含一组可执行的语句，JavaScript将它们一起抛到一个公共的全局名字空间中。

- `var`语句被用在函数内部时，定义的是私有变量。

- 代码块是包在一对花括号中的一组语句，Javascript中的代码块不会创建新的作用域，因此变量应该被定义在函数的头部，而不是代码块中。

- 以下值被当作假：
    - false
    - null
    - undefined
    - 空字符串' '
    - 数字0
    - 数字NaN

- for in语句：枚举一个对象的所有属性名,用object.hasOwnProperty(variable)来确定这个属性名是该对象成员.

```javascript
for(myvar in obj){
    if(obj.hasOwnProperty(myvar)){
    //TODO
    }
}
```


## 对象

- 除了数字、字符串、布尔值、null值和undefined值之外所有的值都是对象。

- Javascript包含一种原型链的特性，允许对象继承另一个对象的属性。

### 对象字面量

- 一个对象字面量就是包围在一对花括号中的零或多个“名/值”对。

```javascript
var person= {
    name: '薛彬',
    age: '24'
}
```

> 用引号括住“first-name”是必须的，但是否括住“first_name”则是可选的。

我怎么感觉这里说反了？

应该是属性名可以不括，而属性值如果不是变量则必须用引号括起来？


```
感觉和json有点像...
```

噢，json中属性名是必须用双引号括起来的，而在对象字面量中是可选的。

### 检索

- 可以采用`[]`或`.`的方式来检索对象里包含的值。在多层对象中，可以通过链式检索。优先考虑`.`表示法，更紧凑且可读性更好。

```javascript
console.log(person.name); //薛彬
console.log(person["age"]); //24
```

- 如果检索的不存在的成员属性的值，则返回`undefined`。
- 利用`||`运算符可以填充默认值。

```javascript
console.log(person.gender || '男');
```

这样并没有填充到实际的对象中。

### 更新

- 可通过赋值语句直接来更新对象里的值，如果属性名存在，这个值将被替换，若不存在，则扩充到对象中。

```javascript
person.gender="男";
```

### 引用

对象通过引用来传递，它们永远不会被复制。

### 原型

每个对象都连接到一个原型对象，并且可以从中继承属性。

### 枚举

- `for in`语句可以用来遍历一个对象中的所有属性名。
- 常见的过滤器是`hasOwnPropety`方法
- 属性名出现的顺序是不确定的，如果想要确保属性以特定的顺序出现，最好的办法就是避免使用`for in`语句，而是创建一个数组。

### 删除

- `delete`运算符可以用来删除对象的属性。如果对象包含该属性，该属性就被移除。

- 删除对象的属性会让来自原型链中的属性透现出来。

```javascript
person.name="张三";
delete person.name;
console.log(person.name);//薛彬
```

### 减少全局变量污染

- 全局变量削弱了程序的灵活性，应该**避免使用**。

## 函数

### 函数对象

- Javascript中函数即是对象，函数对象连接到Function.prototype。
- 函数可以保存在变量、对象和数组中。
- 函数可以被当作参数传递给其他函数，函数也可以返回函数。

### 函数字面量

```javascript
var add=function(a,b){
    return a+b;
}
```

- 函数字面量包括四部分：    
    - 保留字function。
    - 函数名（可省略）。若没有命名，则被称作匿名函数。
    - 参数（用逗号分隔）。这些参数不被初始化为`undefined`，而是在函数被调用时初始化为实际提供的值。
    - 语句。函数的主体，被调用时执行。

- 函数可以被定义在其它函数中。通过函数字面量创建的函数对象包含一个连到外部上下文的连接。这被称为**闭包**。

### 调用

- 调用一个函数会暂停当前函数的执行。
- 每个函数除了声明定义的形参之外，还接受两个附加的参数：this和arguments。
- JavaScript中有4中调用模式：
    - 方法调用模式
    - 函数调用模式
    - 构造器调用模式
    - apply调用模式

### 返回

- 函数从第一句开始执行，在遇到函数体的`}`时结束。
- return语句用来使函数提前返回，不执行余下语句。
- 若没有指定返回值，则返回`undefined`。

### 扩充类型的功能

- 通过给`Object.prototype`添加方法，可以让该方法对所有对象都可用。比如，可以通过给`Function.prototype`增加方法来使得该方法对所有函数可用：

```javascript
Function.prototype.method=function(name,func){
    this.prototype[name]=func;
    return this;
}
```

- JavaScript缺少一个移除字符串首尾空白的方法，可以这样弥补：

```javascript
String.method('trim',function(){
    return this.replace(/^\s+|\s+$/g,'');
})
var str="      哈哈哈    ";
console.log(str);//      哈哈哈    
var result=str.trim();
console.log(result);//哈哈哈
```

基本类型的原型是共用结构，所以在添加方法时要注意是否混用了，覆盖了原有方法，最好的方法时进行一个是否存在的判断：

```javascript
Function.prototype.method=function(name,func){
    if(!this.prototype[name]){
    this.prototype[name]=func;
    }   
    return this;
}
```

### 作用域

- JavaScript不支持块级作用域。
- JavaScript有函数作用域，定义在函数中的参数和变量在函数外部是不可见的，而在函数内部任何位置都可见。
- 建议：在函数体的顶部声明函数中可能用到的所有变量。

### 
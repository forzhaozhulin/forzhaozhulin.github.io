---
layout: post
title:  "Java访问权限修饰"
date:   2016-11-13 14:24:00
categories: Java
tags: Java
author: 薛彬
---
* content
{:toc}

这篇文章主要记录的是《Java编程思想》中关于Java访问权限修饰词这一节内容的笔记。





> `public`、`protected`、`private`这几个Java访问权限修饰词在使用时，当然，还有`friendly`，是置于类中每个成员的定义之前的---无论它是一个域还是一个方法。

先用一个表来表示这四个的访问权限：

|作用域|当前类|同一个包|子类|其它包|
|---|:---|:---|:---|:---|
|public|√|√|√|√|
|protected|√|√|√|×|
|friendly|√|√|×|×|
|private|√|×|×|×|


## 包访问权限

如果不提供任何访问权限修饰词，则意味着它是“包访问权限”，如果要显示表示，就是`friendly`，这是所有成员的默认访问权限。

包访问权限，可能看到这个名字就会觉得，应该是基于`package`来说的，没错。使用了`friendly`来修饰成员时，就意味着当前包中的所有其他类对这个成员都有访问权限，但当前包之外的其它所有类都没有访问权限。

通过代码来看看，假设我们当前包是`com.axuebin`,而还有一个其它包是`com.jack`：

这时候在`com.axuebin`下有一个class为`FriendlyTest.java`:

```java
Class FriendlyTest{
    public static void main(String args[]){
        A a=new A();
        a.f();
    }
}
```

然后在`com.axuebin`下还有一个class为`A.java`:

```java
Class A{
    int a;
    void f(){
    }
}
```

这时候在main方法中是可以使用A类的。

然后如果这个A类是在`com.jack`包下，main中就无法访问A类了，这就是所谓的包访问权限。

值得一提的是，用了`frendly`之后，子孙类也没有访问权限，这也是和`protected`不同的一点。

## public

`public`意思就是公开的，也就是意味着`public`之后紧跟着的成员声明自己对每个人都是可用的。

`public`类中允许有`public`方法，这个方法也可以被所有其它类调用，但是如果`public`类中有其他方法，比如`void f(){}`，这个只是在当前包下提供访问权，在被其它包其它类调用时就无法访问。

一个Java源文件最多只有有一个`public`类，并且文件名和`public`类名保持一致，否则无法编译将会报错。

## private

private是私有的意思，也就是除了包含该成员的类之外，其它任何类都无法访这个成员。

`private`不能用来修饰类。

## protected

`protected`也可以称作继承访问权限，它对于自己的子孙类来说，就是`public`的，可以自由使用没有限制，而对于其他其它包的类来说就变成`private`了。
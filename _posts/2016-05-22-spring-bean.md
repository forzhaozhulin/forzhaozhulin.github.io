---
layout: post
title:  "Spring学习笔记---Bean"
date:   2016-05-22 20:34:00
categories: JavaWeb
tags: Spring
author: 薛彬
---

* content
{:toc}





## 定义

bean 是一个被实例化，组装，并通过 Spring IoC 容器所管理的对象。

bean的属性：

| 属性 | 描述 |
|:---:|:---:|
|class|bean对应的java类|
|name|唯一的bean标识符|
|scope|指定由特定的 bean 定义创建的对象的作用域|
|properties|注入依赖关系|
|……|……|

比如：

```xml
<bean id="helloWorld" class="com.tutorialspoint.HelloWorld">
    <property name="message" value="Hello World!"/>
</bean>
```

## 作用域

新定义一个bean时，必须声明找个bean的作用域。

| 作用域 | 描述 |
|:---:|:---:|
|singleton|该作用域将 bean 的定义的限制在每一个 Spring IoC 容器中的一个单一实例(默认)。|
|prototype|唯一的bean标识符|
|……|……|

### singleton作用域

这是默认的作用域。如果设置成这个作用域，容器会创建一个由该bean定义的对象的实例，该**单一实例**会存储在单例bean的高速缓存中，以及针对该bean的所有后续的请求和引用都返回缓存对象。

```xml
<bean id="..." class="..." scope="singleton"></bean>
```

假设我们现在有这样一个类,里面就是set/get方法：

```java
public class Person{
    private String name;
    public void setName(String name){
        this.name=name;
    }
    public void getName(){
        System.out.println("名字是："+ name);
    }
}
```

还有一个main类：

```java
public class MainApp {
   public static void main(String[] args) {
      ApplicationContext context =new ClassPathXmlApplicationContext("Beans.xml");
      Person obj = (Persond) context.getBean("person");
      //其实就是替代了Person obj = new Person();
      obj.setMessage("张三");
      obj.getMessage();
      Person objB=(Person) context.getBean("Person");
      objB.setMessage("李四");
      objB.getMessage();
   }
}
```

这时候我们的Beans.xml是这样的：

```xml
<bean id="helloWorld" class="com.tutorialspoint.HelloWorld" scope="singleton"></bean>
```

其实就是默认的作用域，运行后会有这样的结果：

![](http://i.imgur.com/9jtGv9S.png)

如果我们把`objB.setMessage("李四");`这句删了，objB会打印出什么呢？

![](http://i.imgur.com/ZIIRlLI.png)

说明obj和objB其实是一个实例，把代码改成这样就会看的更清楚了：

```java
obj.setMessage("张三");
obj.getMessage();

objB.setMessage("李四");
objB.getMessage();

obj.getMessage();
```

最后一条语句是打印什么呢？是`张三`还是`李四`？

![](http://i.imgur.com/JjlWyTM.png)

这就说明了刚开始对于`singleton`的说明，会创建一个**单一实例**。

### prototype作用域

每次特定的bean发出请求时容器就创建新的Bean实例。

>满状态的 bean 使用 prototype 作用域和没有状态的 bean 使用 singleton 作用域。

这句话还没理解。

这时候我们的Beans.xml是这样的：

```xml
<bean id="helloWorld" class="com.tutorialspoint.HelloWorld" scope="prototype"></bean>
```

还是上面的例子：

```java
obj.setMessage("张三");
obj.getMessage();
objB.setMessage("李四");
objB.getMessage();
```

![](http://i.imgur.com/9jtGv9S.png)

同样，我们把`objB.setMessage("李四");`这句删了，objB会打印出什么呢？

![](http://i.imgur.com/DbiXeyh.png)

说明objB是一个新的对象，这个新的对象的Name属性还是null。










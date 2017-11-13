---
layout: post
title:  "Spring学习笔记---@Required注解"
date:   2016-05-23 16:34:00
categories: JavaWeb
tags: Spring
author: 薛彬
---

* content
{:toc}





## 基于注解的配置

使用注解来配置依赖注入，而不用XML来描述一个bean连线。

如果在Spring使用注解，则需要这样配置：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context-3.0.xsd">
	<context:annotation-config/>
	<!-- bean definitions go here -->
</beans>
```

这样Spring就会自动连接值到属性，方法和构造函数。

## @Required注解

@Required注解应用于bean属性的setter方法，它表明影响的bean属性在配置时必须放在XML配置文件中。

拿一个最简单的例子来说明：

main:

```java
public class MainApp {
   public static void main(String[] args) {
      ApplicationContext context = new ClassPathXmlApplicationContext("Beans.xml");
      Person person = (Person) context.getBean("person");
	  person.setName("张三");
      person.setAge(11);
      System.out.println("Name : " + person.getName() );
      System.out.println("Age : " + person.getAge() );
   }
}
```

Person类：

```java
public class Person {
   private Integer age;
   private String name;
   @Required
   public void setAge(Integer age) {
      this.age = age;
   }
   public Integer getAge() {
      return age;
   }
   @Required
   public void setName(String name) {
      this.name = name;
   }
   public String getName() {
      return name;
   }
}
```

Beans.xml:

```xml
<!-- Definition for student bean -->
   <bean id="person" class="com.tutorialspoint.Person">
      <!-- <property name="name"  value="张三" />-->
      <!-- property name="age"  value="11">-->
   </bean>
```

这样运行会有怎样的结果？是不是觉得应该是

```
Name:张三
Age:11
```

实际上这会导致一个`BeanInitializationException`的异常，是因为我们在name和age设置了`@Required`注解，此时的`person`的属性就必须是在XML配置文件下，也就是上面xml中注释的部分。

我们将注释去掉，并且在Main类中将`person.setName("张三");person.setAge(11);`加上注释，这样就可以熟悉的结果了：

![](http://i.imgur.com/h6GaYpW.png)


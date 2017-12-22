---
layout: post
title:  "Spring核心AOP"
date:   2017-12-22
categories: frame
tags: aop
---

* content
{:toc}
## AOP

AOP（Aspect Oriented Programming），即面向切面编程，可以说是OOP（Object Oriented Programming，面向对象编程）的补充和完善。OOP引入封装、继承、多态等概念来建立一种对象层次结构，用于模拟公共行为的一个集合。不过OOP允许开发者定义纵向的关系，但并不适合定义横向的关系，例如日志功能。日志代码往往横向地散布在所有对象层次中，而与它对应的对象的核心功能毫无关系对于其他类型的代码，如安全性、异常处理和透明的持续性也都是如此，这种散布在各处的无关的代码被称为横切（cross cutting），在OOP设计中，它导致了大量代码的重复，而不利于各个模块的重用。

<!-- more -->

AOP技术恰恰相反，它利用一种称为"横切"的技术，剖解开封装的对象内部，并将那些影响了多个类的公共行为封装到一个可重用模块，并将其命名为"Aspect"，即切面。所谓"切面"，简单说就是那些与业务无关，却为业务模块所共同调用的逻辑或责任封装起来，便于减少系统的重复代码，降低模块之间的耦合度，并有利于未来的可操作性和可维护性。

使用"横切"技术，AOP把软件系统分为两个部分：**核心关注点**和**横切关注点**。业务处理的主要流程是核心关注点，与之关系不大的部分是横切关注点。横切关注点的一个特点是，他们经常发生在核心关注点的多处，而各处基本相似，比如权限认证、日志、事务。AOP的作用在于分离系统中的各种关注点，将核心关注点和横切关注点分离开来。

### AOP核心概念

### ![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171222/iDgHj1c8Ie.png?imageslim)

**Spring中AOP代理由Spring的IOC容器负责生成、管理，其依赖关系也由IOC容器负责管理**。因此，AOP代理可以直接使用容器中的其它bean实例作为目标，这种关系可由IOC容器的依赖注入提供。Spring创建代理的规则为：

1、**默认使用Java动态代理来创建AOP代理**，这样就可以为任何接口实例创建代理了

2、**当需要代理的类不是代理接口的时候，Spring会切换为使用CGLIB代理**，也可强制使用CGLIB

AOP编程其实是很简单的事情，纵观AOP编程，程序员只需要参与三个部分：

1、定义普通业务组件

2、定义切入点，一个切入点可能横切多个业务组件

3、定义增强处理，增强处理就是在AOP框架为普通业务组件织入的处理动作

所以进行AOP编程的关键就是定义切入点和定义增强处理，一旦定义了合适的切入点和增强处理，AOP框架将自动生成AOP代理，即：**代理对象的方法=增强处理+被代理对象**的方法。

### 案例：

```java
/**
 * 自定义切面类
 * 
 */
public class MyAspectXml {
	public void checkPrivilege(){
		System.out.println("检测权限...");
	}
}
```

#### 配置文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/aop 
        http://www.springframework.org/schema/aop/spring-aop.xsd">
  
  	 <bean id="customerDao" class="dao.impl.CustomerDaoImpl"></bean>  	
<bean id="myAspectXml" class="aspect.MyAspectXml"></bean>
  	<!-- AOP配置 -->
  	<aop:config>
  		<!-- 配置切入点：告诉spring框架哪些方法需要被增强 -->
  		<aop:pointcut expression="execution(* cn.itcast.dao.impl.CustomerDaoImpl.save(..))" id="pointcut1"/>
  		<!-- 配置切面：告诉spring框架调用切面类中的哪个方法来增强 -->
  		<aop:aspect ref="myAspectXml">
  			<aop:before method="checkPrivilege" pointcut-ref="pointcut1"/>
  		</aop:aspect>
  	</aop:config>
</beans>
```

#### 配置切入点

语法：[修饰符] 返回类型 包名.类名.方法名(形式参数)

常见写法：

- execution(public * *(..))                                     所有的public方法
- execution(* set(..))                                            所有set开头的方法
- execution(* com.xyz.service.AccountService.*(..))             AccountService类中的所有方法
- execution(* com.xyz.service.*.*(..))                            com.xyz.service包下所有的方法
- execution(* com.xyz.service..*.*(..))                     com.xyz.service包及其子包下所有的方法



```xml
<!-- 前置通知 -->
<aop:before method="checkPrivilege" pointcut-ref="pointcut1"/>
<!-- 后置通知 -->
<aop:after-returning method="afterReturn" pointcut-ref="pointcut2" returning="result"/>
<!-- 环绕通知 -->    
<aop:around method="around" pointcut-ref="pointcut3"/>
<!-- 抛出异常通知 -->
<aop:after-throwing method="afterThrowing" pointcut-ref="pointcut4" throwing="ex"/>
<!-- 最终通知 -->
<aop:after method="after" pointcut-ref="pointcut4"/>
```

```java
public class MyAspectXml {

	public void checkPrivilege(JoinPoint point){
		System.out.println("权限校验..." + point);
	}
	
	public void afterReturn(Object result){
		System.out.println("后置通知:" + result);
	}
	/**
     *   joinpoint:正在执行的方法
     */
	public Object around(ProceedingJoinPoint joinpoint){
		System.out.println("环绕前执行");
		Object obj = null;
		try {
			 obj = joinpoint.proceed();//调用正在执行的方法
		} catch (Throwable e) {
			e.printStackTrace();
		}
		System.out.println("环绕后执行");
		return obj;
	}
	
	public void afterThrowing(Exception ex){
		System.out.println("注意了：异常！" + ex.getMessage());
	}
	
	public void after(){
		System.out.println("最终通知");
	}
}

```

参考：<http://www.cnblogs.com/xrq730/p/4919025.html>
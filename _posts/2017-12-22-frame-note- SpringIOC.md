---
layout: post
title:  "Spring-IOC和DI"
date:   2017-12-22
categories: frame
tags: Spring IOC DI 控制反转 依赖注入
---

* content
{:toc}
## Spring

Spring是一个开放源代码的设计层面框架，他解决的是业务逻辑层和其他各层的松耦合问题。Spring提供了JavaEE各层的解决方案，表现层：Spring MVC，持久层：JdbcTemplate、ORM框架整合，业务层：IoC、AOP、事务控制。

<!-- more -->

### 为什么要学习Spring

#### ![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171222/d1F2J6h3Kk.png?imageslim)

## IOC：

它是Inverse of Control，控制反转，将对象的创建权力反转给Spring框架！！

解决问题：使用IOC可以解决的程序耦合性高的问题！！

 原理：

​    service有dao的引用，所以两者之间有非常强的耦合性。dao的实现一旦发生改变，service层必须要修改代码。这不符合里氏替换原则。所以我们需要解耦。IOC使用工厂类来创造dao的实例，这样当dao的实现改变时无需改变service的代码，但仍需修改工厂类代码。使用工厂类+配置文件的方式，当需要更改dao时，只需要更改配置文件就行，不用更改源代码，程序彻底解耦了。

在src下创建配置文件applicationContext.xml：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<bean id="userDao" class="dao.impl.daoImpl"></bean>
```

工厂类:

```java
public class BeanFactory {
	
	//存放所有的对象
	private static final Map<String,Object> map = new HashMap<String,Object>();
	
	static{
		SAXReader sax = new SAXReader();
		try {
			Document document = sax.read(BeanFactory.class.getClassLoader().getResourceAsStream("applicationContext.xml"));
			Element root = document.getRootElement();
			//获取id属性的值
			String id = root.attributeValue("id");
			//获取class属性的值
			String value = root.attributeValue("class");
			//根据class属性的值实例化对象
			Object obj = Class.forName(value).newInstance();
			//把对象存放到Map中
			map.put(id, obj);
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
	
	/**
	 * 根据id获取对象
	 * @param id
	 * @return
	 */
	public static Object getBean(String id){
		return map.get(id);
	}
}
```

### Spring配置文件

##### id属性:

id属性是bean的唯一标识

##### class属性:

bean的全路径名

##### scope属性:

scope属性代表Bean的作用范围

- singleton:单例（默认值）
- prototype:多例，在Spring框架整合Struts2框架的时候，Action类也需要交给Spring做管理，配置把Action类配置成多例

当scope为prototype时，每次获取bean，都会重新实例化.

##### init-method属性

当bean被载入到容器的时候调用init-method属性指定的方法

destory-method属性

当bean从容器中删除的时候调用destroy-method属性指定的方法

### Spring生成bean:

1. 无参构造方法

   ​    默认调用无参构造方法实例化bean

2. 静态工厂实例化方式

   ​    通过调用工厂类的静态方法来生成bean

 ```java
public interface DepartmentDao {

	public void save();
}
public class DepartmentDaoImpl implements DepartmentDao{

	@Override
	public void save() {
		System.out.println("部门保存...");
	}

}
public class DepartmentDaoFactory {

	/**
	 * 静态工厂方法获取对象
	 * @return
	 */
	public static DepartmentDao create(){
		return new DepartmentDaoImpl();
	}
}
 ```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
        <bean id="userDao" class="cn.itcast.dao.impl.UserDaoImpl" init-method="init" destroy-method="destory"/>
        <bean id="departmentDao" class="cn.itcast.dao.impl.DepartmentDaoFactory" factory-method="create"></bean>
</beans>
```

3. 实例工厂

```java
public class DepartmentDaoFactory {	
	/**
	 * 实例工厂方法获取对象
	 * @return
	 */
	public DepartmentDao create1(){
		return new DepartmentDaoImpl();
	}
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
        <bean id="userDao" class="cn.itcast.dao.impl.UserDaoImpl" init-method="init" destroy-method="destory"/>
       <!--  <bean id="departmentDao" class="cn.itcast.dao.impl.DepartmentDaoFactory" factory-method="create"></bean> -->
        <bean id="departmentDaoFactory" class="cn.itcast.dao.impl.DepartmentDaoFactory" ></bean>
        <bean id="departmentDao" factory-bean="departmentDaoFactory" factory-method="create1"></bean>
</beans>
```

## 依赖注入:

IOC       -- Inverse of Control，控制反转，将对象的创建权反转给Spring

DI        -- Dependency Injection，依赖注入，在Spring框架负责创建Bean对象时，动态的将依赖对象注入到Bean组件中

如果Service的实现类ServiceImpl中有一个属性，那么使用Spring框架的IOC功能时，可以通过依赖注入把该属性的值传入进来

#### 构造方法注入

```java
public class Car implements Serializable{
private static final long serialVersionUID = 1L;
	private String name;
	private Double price;
	
	public Car(String name, Double price) {
		this.name = name;
		this.price = price;
	}
    // get set 方法
}
```

配置applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
        <bean id="userDao" class="cn.itcast.dao.impl.UserDaoImpl" />
        <bean id="departmentDaoFactory" class="dao.impl.DepartmentDaoFactory" ></bean>
        <bean id="departmentDao" factory-bean="departmentDaoFactory" factory-method="create1"></bean>
        <!-- 构造方法注入 -->
        <bean id="car" class="domain.Car">
        	<constructor-arg name="name" value="奥迪A6"></constructor-arg>
        	<constructor-arg name="price" value="57.3"></constructor-arg>
        </bean>
</beans>
```

#### 方法注入

```java
public class Student implements Serializable {

	private static final long serialVersionUID = 1L;
	
	private Long id;
	private String name;
    // get set 方法
}
```

配置applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
        <bean id="userDao" class="dao.impl.UserDaoImpl" />
        <bean id="departmentDaoFactory" class="dao.impl.DepartmentDaoFactory" ></bean>
        <bean id="departmentDao" factory-bean="departmentDaoFactory" factory-method="create1"></bean>
        <!-- set方法注入 -->
        <bean id="student" class="domain.Student">
        	<property name="id" value="11"></property>
        	<property name="name" value="张三"></property>
        </bean>
</beans>
```

#### set方法注入对象

```java
public class People implements Serializable {

	private static final long serialVersionUID = 1L;
	private Long id;
	private String name;
	private Car car;
    
    // get set 方法
}
```

配置applicationContext.xml

```xml
<!-- set方法注入对象 -->
 <bean id="people" class="domain.People">
       <property name="name" value="小明"></property>
       <property name="car" ref="car"></property>
 </bean>

```

#### 集合注入：

```xml
<bean id="collectionBean" class="cn.itcast.domain.CollectionBean">
        	<property name="array">
        		<list>
        			<value>威少</value>
        			<value>哈登</value>
        			<value>莱昂纳德</value>
        		</list>
        	</property>
            <property name="set">
        		<set>
        			<value>苹果</value>
        			<value>梨子</value>
        			<value>香蕉</value>
        		</set>
        	</property>
            <property name="map">
        		<map>
        			<entry key="id" value="11"></entry>
        			<entry key="name" value="张三"></entry>
        			<entry key="age" value="20"></entry>
        		</map>
        	</property>
            <property name="props">
        		<props>
        			<prop key="username">李四</prop>
        			<prop key="password">123</prop>
        		</props>
        	</property>
</bean>
```
---
layout: post
title:  "file"
date:   2017-12-12
categories: frame
tags: 
---

* content
{:toc}
### 主键生成策略：

自然主键和代理主键：

自然主键（业务主键）：把具有业务含义的字段作为主键

代理主键（逻辑主键）:  不具备业务含义的字段作为主键该字段一般取名为“ID”

<!-- more -->

### ![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171212/CL0ab72GcB.png?imageslim)

#### 

### Hibernate对象状态:

1、瞬时态（transient）

​	不存在持久化标识OID（相当于主键值），尚未与Hibernate Session关联

2、持久态（persistent）

​	存在持久化标识OID ，加入到了Session缓存中，并且相关联的Session没有关闭，在数据库中有对应的记录

3、脱管态（detached）

​	脱管态对象存在持久化标识OID，并且仍然与数据库中的数据存在关联，只是失去了与当前Session的关联

### Hibernate的事务控制:

要保证在Service中开启的事务时使用的Session对象和DAO中多个操作使用的是同一个Session对象

使用ThreadLocal将业务层获取的Session绑定到当前线程中，然后在DAO中获取Session的时候，都从当前线程中获取

由于hibernate早就考虑到了事务，所以hibernate内部已经封装了相关事务的事务的方法

我们只需要在配置文件中开启即可；

在hibernate.cfg.xml文件中配置同时一般还配置事务的隔离级别：

```xml
<!-- 把session绑定到当前线程上 -->
<property name="hibernate.current_session_context_class">thread</property>
<!--
 事务隔离级别 
hibernate.connection.isolation = 4 
1—Read uncommitted isolation （读取未提交的内容。  最低级别，任何情况都无法保证）
2—Read committed isolation （读取提交的内容。   避免脏读的发生）
4—Repeatable read isolation （可重复读。   避免脏读和可重复读的发生）
8—Serializable isolation （可串行化。   最高级别，避免脏读、可重复读和幻读的发生）
-->
<property name="hibernate.connection.isolation">4</property>
```

### **Query对象（QHL）：**

Query代表面向对象的一个Hibernate查询操作。在Hibernate中，通常使用session.createQuery()方法接受一个HQL语句，然后调用Query的list()或uniqueResult()方法执行查询。所谓的HQL是Hibernate Query Language缩写，其语法很像SQL语法，但它是完全面向对象的。

在Hibernate中使用Query对象的步骤，具体所示：

（1）获得Hibernate的Session对象。

（2）编写HQL语句。

（3）调用session.createQuery 创建查询对象。

（4）如果HQL语句包含参数，则调用Query的setXxx设置参数。

（5）调用Query对象的方法执行查询。

HQL**说明：把表的名称换成实体类名称。把表字段名称换成实体类属性名称。**

**例如：**

SQL：select * from cst_customer where cust_name like ?

HQL：select c from Customer c where custName like ?

​    

其中select c 可以省略，写为：from Customer where custName like ?

注：下列所有代码均在事务中执行

```java
		//获取Query对象
		Query query =session.createQuery("from Customer");
		//通过Query对象的方法，获取结果集
		List list =query.list();
		//遍历集合，取出并输出对象
		for(Object obj : list){
			System.out.println(obj);
		}

```

条件查询:

```java
//获取Query对象
		Query query =session.createQuery("from Customer where custName like ? and custLevel = ?");
		//Hibernate中的参数占位符索引是从0开始
		query.setString(0, "%集团%");//给第一个参数占位符赋值
		query.setString(1, "普通客户");//给第二个参数占位符赋值
		//调用Query对象的方法，获取结果集
		List list = query.list();
		//遍历结果集
		for(Object obj : list){
			System.out.println(obj);
		}
```

分页：

```java
		//获取Query对象
		Query query =session.createQuery("from Customer");
		//Hibernate中设置分页的方法
		query.setFirstResult(0);//设置记录的索引
		query.setMaxResults(2);//设置每次查询的记录条数
		//调用Query对象的方法，获取结果集
		List list = query.list();
		//遍历结果集
		for(Object obj : list){
			System.out.println(obj);
		}	
```

**Query对象中的方法说明**

​    list方法：该方法用于查询语句，返回的结果是一个list集合。

​    uniqueResult方法：该方法用于查询，在确保只有一条记录的查询时可以使用该方法，返回的结果是一个Object对象。



### Criteria对象（QBC）

Criteria是一个完全面向对象，可扩展的条件查询API，通过它完全不需要考虑数据库底层如何实现，以及SQL语句如何编写，它是Hibernate框架的核心查询对象。

使用Criteria对象查询数据的主要步骤:

（1）获得Hibernate的Session对象。

（2）通过Session获得Criteria对象。

（3）使用Restrictions的静态方法创建Criterion条件对象。Restrictions类中提供了一系列用于设定查询条件的静态方法，这些静态方法都返回Criterion实例，每个Criterion实例代表一个查询条件。

（4）向Criteria对象中添加Criterion 查询条件。Criteria的add()方法用于加入查询条件。

（5）执行Criterita的 list() 或uniqueResult() 获得结果。

```java
     //创建Criteria对象
		Criteria c =session.createCriteria(Customer.class);//相当于HQL的from Customer
		//执行Criteria对象的方法，获取结果集
		List list =c.list();
		//遍历
		for(Object obj : list){
			System.out.println(obj);
		}

```

条件查询：

```java
        //创建Criteria对象
		Criteria c =session.createCriteria(Customer.class);//相当于HQL的from Customer
		//设置查询条件
		c.add(Restrictions.like("custName", "%集团%"));//相当于where custName like '%集团%'
		c.add(Restrictions.eq("custLevel", "普通客户"));//相当于and custLevel ='普通客户'
		//执行Criteria对象的方法，获取结果集
		List list =c.list();
		//遍历
		for(Object obj : list){
			System.out.println(obj);
		}
```

分页查询和HQL一摸一样

**统计查询：**

```java
        //创建Criteria对象
		Criteria c =session.createCriteria(Customer.class);//相当于HQL的from Customer
		// 调用
		//c.setProjection(Projections.rowCount());//相当于 select count(*)
		c.setProjection(Projections.count("custId"));//相当于 select count(cust_id) 
		//调用仅返回唯一结果的方法
		Long count = (Long)c.uniqueResult();
		System.out.println("公司现有客户"+count+"个");
```


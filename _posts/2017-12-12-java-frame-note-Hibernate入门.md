---
layout: post
title:  "Hibernate快速入门"
date:   2017-12-12
categories:  frame
tags: Hibernate ORM HibernateUtil
---

* content
{:toc}
##Hibernate简述

Hibernate框架是主流的Java持久层（Dao层）的框架之一。

Hibernate是一个开放源代码的ORM框架，它对JDBC进行了轻量级的对象封装，使得Java开发人员可以使用面向对象的编程思想来操作数据库。

<!-- more -->

使用传统的JDBC开发应用系统时，如果是小型应用系统，并不觉得有什么麻烦，但是对于大型应用系统的开发，使用JDBC就会显得力不从心。

例A：对几十、几百张包含几十个字段的表进行插入操作时，编写的SQL语句不但很长，而且繁琐，容易出错；

例B：在读取数据时，需要写多条getXxx语句从结果集中取出各个字段的信息，不但枯燥重复，并且工作量非常大。

Hibernate具有以下几点优势：

1. Hibernate对JDBC访问数据库的代码做了轻量级封装，大大简化了数据访问层繁琐的重复性代码，并且减少了内存消耗，加快了运行效率。
2. Hibernate是一个基于JDBC的主流持久化框架，是一个优秀的ORM实现，它很大程度的简化了DAO（Data Access Object，数据访问对象）层编码工作。
3. Hibernate的性能非常好，映射的灵活性很出色。它支持很多关系型数据库，从一对一到多对多的各种复杂关系。
4. 可扩展性强，由于源代码的开源以及API的开放，当本身功能不够用时，可以自行编码进行扩展。



#### **ORM概述**

Object Relation Mapping 对象关系映射。

ORM技术是在对象和关系之间提供了一条桥梁，前台的对象型数据和数据库中的关系型的数据通过这个桥梁来相互转化。

ORM 就是把Java的实体类和数据表关联起来直接操作Java中的实体类就相当于操作数据库.

## 快速入门

Hibernate使用普通Java对象（Plain Old Java Object），即POJO的编程模式来进行持久化。POJO类中包含的是与数据库表相对应的各个属性，这些属性通过getter和setter方法来访问，对外部隐藏了内部的实现细节。

**POJO类其实就是实体类。**

**数据表：**

```java
/*创建客户表*/
CREATE TABLE `cst_customer` (
  `cust_id` bigint(32) NOT NULL AUTO_INCREMENT COMMENT '客户编号(主键)',
  `cust_name` varchar(32) NOT NULL COMMENT '客户名称(公司名称)',
  `cust_source` varchar(32) DEFAULT NULL COMMENT '客户信息来源',
  `cust_industry` varchar(32) DEFAULT NULL COMMENT '客户所属行业',
  `cust_level` varchar(32) DEFAULT NULL COMMENT '客户级别',
  `cust_address` varchar(128) DEFAULT NULL COMMENT '客户联系地址',
  `cust_phone` varchar(64) DEFAULT NULL COMMENT '客户联系电话',
  PRIMARY KEY (`cust_id`)
) ENGINE=InnoDB AUTO_INCREMENT=94 DEFAULT CHARSET=utf8;
```

**客户实体类：**

```java
public class Customer implements Serializable {
	private Long custId;// 客户编号
	private String custName;// 客户名称
	private String custSource;// 客户信息来源
	private String custIndustry;// 客户所属行业
	private String custLevel;// 客户级别
	private String custAddress;// 客户联系地址
	private String custPhone;// 客户联系方式
    //省略get set 方法
}
```

**`映射配置文件`(xml) ：**

Hibernate需要知道实体类Customer映射到数据库中的哪个表，以及类中的哪个属性对应数据库表中的哪个字段，这些都需要在映射文件中配置。在实体类Customer所在的包中。一般命名为XXX.hbm.xml

Customer.hbm.xml：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-mapping PUBLIC 
    "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
    "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">
<hibernate-mapping package="cn.itcast.domain"><!-- package属性用于设定包的名称，接下来该配置文件中凡是用到此包中的对象时都可以省略包名 -->
	<!-- class标签
			作用：建立实体类和表的对应关系
			属性：
				name：指定实体类的名称
				table：指定数据库表的名称
	 -->
	 <class name="Customer" table="cst_customer">
	    <!-- id标签
			 作用：用于映射主键
			 属性：
			 	name：指定的是属性名称。也就是get/set方法后面的部分，并且首字母要转小写。
			 	column:指定的是数据库表的字段名称
		-->
	    <id name="custId" column="cust_id">
	        <!-- generator标签：
			作用：配置主键的生成策略。
			属性：
			  class:指定生成方式的取值。
			  取值之一：native   使用本地数据库的自动增长能力。
			  mysql数据库的自动增长能力是让某一列自动+1。但是不是所有数据库都支持这种方式
		   -->
	       <generator class="native"></generator>
	    </id>
	    <!-- property标签：
				作用：映射其他字段
				属性：
					name：指定属性的名称。和id标签的name属性含义一致
					column：指定数据库表的字段名称
		-->
	    <property name="custName" column="cust_name"></property>
	    <property name="custSource" column ="cust_source"></property>
	    <property name="custIndustry" column ="cust_industry"></property>
	    <property name="custLevel" column ="cust_level"></property>
	    <property name="custAddress" column ="cust_address"></property>
	    <property name="custPhone" column ="cust_phone"></property>
	 </class>
</hibernate-mapping>
```

若实体类的变量名和表中的字段相同，那么以上xml中的column属性均可省略.

**主配置文件(hibernate.cfg.xml):**

Hibernate的映射文件(例:Customer.hbm.xml)反映了持久化类和数据库表的映射信息，而Hibernate的配置文件则主要用来配置数据库连接以及Hibernate运行时所需要的各个属性的值。hibernate.cfg.xml文件创建在项目的src下。

**Hibernate.cfg.xml：**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-configuration PUBLIC
	"-//Hibernate/Hibernate Configuration DTD 3.0//EN"
	"http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">
<hibernate-configuration>
  <!-- 配置SessionFactory 
	SessionFactory就是一个工厂，用于生产Session对象的。
	Session就是我们使用hibernate操作数据库的核心对象了。
		创建SessionFactory由三部分组成：(缺一不可)
		1、连接数据库的基本信息
		2、hibernate的基本配置
		3、映射文件的位置
		找配置文件的key都是在hibernate的开发包中project文件夹下的etc目录中的hibernate.properties
		路径：...\hibernate-release-5.0.7.Final\project\etc\hibernate.properties
  -->
  <session-factory>  
  <!-- 1、数据库的基本信息 -->
		<property name="hibernate.connection.driver_class">com.mysql.jdbc.Driver</property>
		<property name="hibernate.connection.url">jdbc:mysql://localhost:3306/test</property>
		<property name="hibernate.connection.username">root</property>
		<property name="hibernate.connection.password">root</property>
		<!-- 数据库的方言 -->
		<!-- 新版本的数据库方言：org.hibernate.dialect.MySQL5Dialect -->
		<property name="hibernate.dialect">org.hibernate.dialect.MySQL5Dialect</property>
  <!-- 2、hibernate的基本配置 -->
		<!-- 是否显示SQL语句 -->
		<property name="hibernate.show_sql">true</property>
		<!-- 是否格式化SQL语句 -->
		<property name="hibernate.format_sql">true</property> 
  <!-- 3、映射文件的位置 -->
		<mapping resource="cn/itcast/domain/Customer.hbm.xml"/>
  </session-factory>
</hibernate-configuration>
```

**CustomerDao类：**

```java
public class CustomerDao {
   public static void save(Customer c){
	        /*Configuration config = new Configuration();//默认加载Hibernate.properties 要手动加载映射文件*/		
		    Configuration config = new Configuration().configure();//默认加载Hibernate.cfg.xml
		    /*config.addResource("cn/itcast/domain/Customer.hbm.xml");//手动加载*/
	 		//创建SessionFactory对象
	 		SessionFactory factory=cfg.buildSessionFactory();
	 		//使用SessionFactory生产一个Session
	 		Session session=factory.openSession();
	 		//开启事务
	 		Transaction tx=session.beginTransaction();
	 		//保存客户
	 		session.save(c);
	 		//提交事务
	 		tx.commit();
	 		
	 		session.close();
	 		factory.close();
   }
}
```



### 案例的运行流程：

先创建Configuration的实例，由他读取配置文件hibernate.cfg.xml。然后创建SessionFactory读取Configuration对象中所有的信息。在打开Session 让SessionFactory提供连接，开启一个事务，之后创建对象，向对象添加数据，通过session.save()方法完成向数据库中保存数据的操作。最后提交事务，并关闭资源。

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171212/jj1le540Bg.png?imageslim)



### Hibernate的常见配置讲解

**映射文件的配置**

之前案例中使用过的Customer.hbm.xml文件，该文件用于向Hibernate提供持久化类到关系型数据库的映射，每个映射文件的的结构基本都是相同的，其普遍的代码形式如下所示。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 映射文件的DTD信息 -->
<!DOCTYPE hibernate-mapping PUBLIC 
    "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
    "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">
<!--
hibernate-mapping标签  属于映射文件的顶层标签
属性：package
用于设定包的名称，接下来该配置文件中凡是用到此包中的对象时都可以省略包名 
-->
<hibernate-mapping package="包名">
     <!-- class标签   作用：建立实体类和表的对应关系 
          name 代表的是实体类名称
          table 代表的是数据库表名称
-->
	 <class name="Xxx"  table="xxx">
<!-- id标签   作用：用于映射主键
name  代表的是Xxx类中的属性名
	     column  代表的是xxx表中的字段名
-->
	    <id name="Id" column="id">
	        <!-- generator是主键生成策略 -->
	       <generator class="native"></generator>
	    </id>
	    <!-- property标签  作用：映射其他非主键字段 -->
	    <property name="实体类属性名" column="数据表字段名" ></property>
	 </class>
</hibernate-mapping>

```

映射文件通常是一个XML文件即可,但一般命名为：类名.hbm.xml

**核心文件的配置**

Hibernate的核心配置文件（hibernate.cfg.xml），包含了连接持久层与映射文件所需的基本信息，其配置文件有两种格式，具体如下：

* 一种是properties属性文件格式的配置文件，它使用键值对的形式存放信息，默认文件名称为hibernate.properties
* 另一种是XML格式的配置文件，XML配置文件的默认名称为hibernate.cfg.xml

上述两种格式的配置文件是等价的，具体使用哪个可以自由选择。 在这里向大家推荐使用XML格式的，XML格式的配置文件更易于修改，配置能力更强，当改变底层应用配置时不需要改变和重新编译代码，只修改配置文件的相应属性即可，而properties格式的文件则不具有此优势，因此，在实际开发项目中，大多数情况会使用XML格式的配置文件。下面将对XML格式的配置文件进行详细介绍。

hibernate.cfg.xml配置文件一般在开发时会放置在类的根路径下(现在是放置在src文件夹下)。配置文件的常用配置信息如下所示。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- DTD信息：用来标记约束 -->
<!DOCTYPE hibernate-configuration PUBLIC
	"-//Hibernate/Hibernate Configuration DTD 3.0//EN"
	"http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">
<!-- Hibernate相关配置信息 -->
<hibernate-configuration>
<!-- 配置Session工厂信息（全局） -->
  <session-factory>  
  <!-- 必要的配置信息1 : 四本一言(4个基本项，1个方言)-->
<!-- 4个基本项：四个连接数据库的基本参数 -->
<!-- 数据库驱动 -->
	<property name="hibernate.connection.driver_class">
com.mysql.jdbc.Driver
</property>
<!-- 数据库连接字符串 -->
	<property name="hibernate.connection.url">
jdbc:mysql://localhost:3306/test
</property>
     <!-- 数据库登录名 -->
	<property name="hibernate.connection.username">root</property>
<!-- 数据库登录密码 -->
	<property name="hibernate.connection.password">root</property>
     <!-- 数据库的方言 : 根据配置的方言生成相应的SQL语句 -->
	<property name="hibernate.dialect">
org.hibernate.dialect.MySQL5Dialect
</property>
  <!-- 必要的配置信息2 : hibernate的基本属性 -->
	<!-- 是否显示SQL语句  在控制台打印输出SQL语句-->
	<property name="hibernate.show_sql">true</property>
	<!-- 是否格式化SQL语句 -->
	<property name="hibernate.format_sql">true</property>

<!-- Hibernate的hbm2ddl(数据定义语言:create drop alter ...)属性 -->
<!--	hibernate可以根据映射文件来为我们生成数据库的表结构（但是不能生成数据库）
		hbm2ddl.auto的取值
		* none:不用Hibernate自动生成表
		* create:每次都会创建一个新的表(测试)
		* create-drop:每次都会创建一个新的表，执行程序结束后删除这个表(测试)
		* update:如果数据库中有表，使用原来的表，如果没有表，创建一个新表.可以更新表结构
		* validate:只会使用原有的表.对映射关系进行校验
	-->
	<property name="hibernate.hbm2ddl.auto">update</property>

  <!-- 必要的配置信息3 : 映射文件加载 -->
		<mapping resource="cn/itcast/domain/Customer.hbm.xml"/>
  </session-factory>
</hibernate-configuration>

```

在上述代码中，首先进行了xml声明，然后是配置文件的dtd信息，该信息同样可以在核心包hibernate-core-5.0.7.Final.jar下的org.hibernate包中的hibernate-configuration-3.0.dtd文件中找到，只需要复制过来用即可，不需要刻意记忆。

Hibernate配置文件的根元素是hibernate-configuration，该元素包含子元素session-factory，在session-factory元素中又包含多个property元素，这些property元素用来对Hibernate连接数据库的一些重要信息进行配置。例如，上面的配置文件中，使用了property元素配置了数据库的方言、驱动、URL、用户名、密码等信息。最后通过mapping元素的配置，加载出映射文件的信息。

Hibernate常用配置属性



![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171212/7B68g3gd5c.png?imageslim)

### SessionFactory:

接口负责Hibernate的初始化和建立Session对象

SessionFactory对象维护了很多信息：

1.  Hibernate主配置文件中的基本配置
2.  映射文件的位置，以及映射文件中的配置
3.  映射文件所对应表的一些预定义的SQL语句（这些语句都是通用的） 比如：全字段保存，根据id的全字段更新，根据id的全字段查询，根据id的删除等等。

由于SessionFactory维护了很多信息同时又是线程安全的，一般情况下，一个项目中只需要一个SessionFactory，只有当应用中存在多个数据源时，才为每个数据源建立一个SessionFactory实例。因此，不应该反复的创建和销毁。

## 抽取HibernateUtil工具类

将Configuration、SessionFactory、Session这些对象抽取出来创建一个工具类，用来提供Session对象.

```java
public class HibernateUtil {
	private static SessionFactory factory;
	// 使用静态代码块获取SessionFactory
	static {
		//细节：Hibernate把所有可预见的异常，都转成了运行时异常（工具中不会提示要添加异常块）
		try {
			// 加载hibernate.cfg.xml配置文件
			Configuration config = new Configuration().configure();
			factory = config.buildSessionFactory();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
	/**
	 * 获取一个新的Session对象（每次都是一个新对象）
	 * @return
	 */
	public static Session openSession(){
		return factory.openSession();
	}
}
```

### Hibernate查询客户信息:

1、保存一个客户到数据库的客户表中     save()

2、从数据库中查询新增的客户     get()

```java
public class CustomerDao {
	/**
	 * 查询新增客户信息
	 * 
	 * @param c
	 */
	public static void findOne(Customer c){
		Session session = null;//Session对象
		Transaction tx = null ;//事务对象
		try{
			//使用Hibernate工具类获取Session对象
			session = HibernateUtil.getOpenSession();
			tx = session.beginTransaction();//开启事务
			//保存客户
			Serializable id = session.save(c);//save方法返回实体保存后的主键值
			System.out.println("新增客户ID："+id);
			//根据客户的ID(主键)查询客户信息
			Customer newCustomer = session.get(Customer.class, id);
			//提交事务
			tx.commit();
			System.out.println(newCustomer);
		}catch(Exception e){
			tx.rollback();//事务回滚
			e.printStackTrace();
		}finally{
			session.close();//释放资源
		}
	}
｝
```

```java
public class HibernateDemo1 {
	@Test
	public void test2(){
		//创建客户类对象
		Customer c= new Customer();
		c.setCustName("张三");
		
		CustomerDao.findOne(c);
	}
}
```

### Hibernate修改客户信息:

1、通过客户ID查询客户信息 get()

2、对查询出的客户手机号码进行修改 update()

```java
public class CustomerDao {
	/**
	 * 通过ID查询一个客户信息
	 * @param id
	 * @return
	 */
	public static Customer findOneById(Long id){
		Customer c =null;
		Session session = null;
		Transaction tx = null ;
		try{
			session = HibernateUtil.getOpenSession();
			tx = session.beginTransaction();
			tx.commit();
		}catch(Exception e){
			tx.rollback();
			e.printStackTrace();
		}finally{
			session.close();
		}
		return c;
	}
	/**
	 * 修改客户手机号
	 * @param c
	 * @param newPhone
	 */
	public static void update(Customer c, String newPhone){
		Session session = null;
		Transaction tx = null ;
		try{
			session.update(c);
			tx.commit();
		}catch(Exception e){
			tx.rollback();
			e.printStackTrace();
		}finally{
			session.close();
		}
	}
｝
```

```java
public class HibernateDemo1 {
	@Test
	public void test3(){
		//通过ID查询到Customer实体对象
		Customer c = CustomerDao.findOneById(94L);
		//修改所查询到Customer实体对象中的手机号码
		CustomerDao.update(c, "13899990000");
	}
}
```

### Hibernate删除客户信息:

1、根据客户编号查询客户实体 get()

2、删除客户信息 delete()

```java
public class CustomerDao {
	public static void delete(Customer c){
		Session session = null;
		Transaction tx = null ;
		try{
			tx.commit();
		}catch(Exception e){
			tx.rollback();
			e.printStackTrace();
		}finally{
			session.close();
		}
	}
	public static Customer findOneById(Long id){
		Customer c =null;
		Session session = null;
		Transaction tx = null ;
		try{
			tx.commit();
		}catch(Exception e){
			tx.rollback();
			e.printStackTrace();
		}finally{
			session.close();
		}
		return c;
	}
}
```

```java
public class HibernateDemo1 {
	@Test
	public void test4(){
		//通过ID查询Customer实体
		Customer c = CustomerDao.findOneById(95L);
		//删除客户实体
		CustomerDao.delete(c);
	}
}
```

（delete方法默认是根据主键删除）














































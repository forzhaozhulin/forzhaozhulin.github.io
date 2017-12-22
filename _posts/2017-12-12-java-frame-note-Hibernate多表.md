---
layout: post
title:  "Hibernate多表"
date:   2017-12-12
categories: frame
tags: Hibernate多表
---

* content
{:toc}
##  Hibernate多表

在实际中，数据库的表难免会有相互的关联关系，在操作表的时候就有可能会涉及到多张表的操作。

hibernate实现了ORM的思想。让我们通过实体类操作数据表。

<!-- more -->

### 实现多表映射的步骤：

1. 确定两张表的关系
2. 在数据库中实现两张表的关系
3. 在实体类中描述出两个实体的关系
4. 配置出实体类和数据库表的关系映射

### 一对多关系映射：

**客户实体类（一方）**

```java
public class Customer implements Serializable {
	private Long custId;
	private String custName;
	private String custSource;
	private String custIndustry;
	private String custLevel;
	private String custAddress;
	private String custPhone;
	
	//一对多关系映射：一个客户可以对应多个联系人
	private Set<LinkMan> linkmans=new HashSet<LinkMan>();
	public Set<LinkMan> getLinkmans() {
		return linkmans;
	}
    //get,set方法
}
```

**联系人实体类（多方）**

```java
public class LinkMan implements Serializable {
	private Long lkmId;
	private String lkmName;
	private String lkmGender;
	private String lkmPhone;
	private String lkmMobile;
	private String lkmEmail;
	private String lkmPosition;
	private String lkmMemo;
	
	//多对一关系：多个联系人对应客户
	private Customer customer;//对应联系人表中的外键
	public Customer getCustomer() {
		return customer;
	}
    //get,set方法
}
```

**客户配置文件：**

**在class中加入下列代码**

```xml
<!-- 一对多关系映射 
         set标签：用于映射set集合属性
		属性：
			name：指定集合属性的名称
		     table：指定的是集合元素所对应的表（在一对多的时候写不写都可以）

	     key标签：用于映射外键字段的
		属性：
			column：指定从表中的外键字段名称

		one-to-many标签：用于指定当前映射配置文件所对应的实体和集合元素所对应的实体是一对多关系。
		属性：
		     class：指定集合元素所对应的实体类名称
		-->
	    <set name="linkmans" table="cst_linkman">
	        <key column="lkm_cust_id"></key>
	        <one-to-many class="domain.LinkMan"/>
	    </set>
```

**联系人配置文件：**

```xml
<!-- 多对一关系映射 
	   many-to-one标签：用于建立多对一的关系映射配置
	   属性：
		  name：指定的实体类中属性的名称
		  class：该属性所对应的实体类名称。如果在hibernate-mapping上没有导包，则需要写全限定类名
		  column：指定从表中的外键字段名称
	   -->
       <many-to-one name="customer" class="domain.Customer" column="lkm_cust_id"></many-to-one>
```

**注意：**在设置过Customer.hbm.xml文件和LinkMan.hbm.xml文件后，需要在hibernate.cfg.xml中引入两个映射文件位置

```xml
<mapping resource = "domain/Customer.hbm.xml"/>
<mapping resource = "domain/LinkMan.hbm.xml"/>
```

### 外键维护权

当我们建立了双向的关联关系之后，先保存主表，再保存从表时：会产生2条insert和1条update。 而实际开发中我们只需要2条insert，持久态对象可以自动更新数据库，更新客户的时候会修改一次外键，更新联系人的时候同样也会修改一次外键。这样就会产生了多余的SQL。

这时就需要一方放弃维护外键维护权

**在一对多中，一的一方都会放弃外键的维护权**（关系的维护）。

配置文件修改如下：

```xml
<!-- 一对多关系映射 -->
	    <set name="linkmans" table="cst_linkman" inverse="true">
	        <key column="lkm_cust_id"/>
	        <one-to-many class="cn.itcast.domain.LinkMan"/>
	    </set>
```

### 级联

由于Customer放弃了外键维护权，对Customer对象进行更新时，仅是操作Customer对象，而Customer对象关联的其它对象(LinkMan对象)不会有任何操作。

这时需要配置级联操作：

**什么是级联操作：**级联操作是指当主控方执行保存、更新或者删除操作时，其关联对象（被控方）也执行相同的操作

**级联操作的方向性：**级联是有方向性的，所谓的方向性指的是，在保存一的一方级联多的一方，或者是在保存多的一方级联一的一方。配置级联操作，首先要确定我们要保存的主控方是那一方，在我们的案例中是要保存客户，所以客户是主控方，那么需要在客户的映射文件中进行如下的配置：

```xml
   <!-- 一对多关系映射 -->
	    <set name="linkmans" table="cst_linkman" cascade="save-update" inverse="true">
	        <key column="lkm_cust_id"/>
	        <one-to-many class="cn.itcast.domain.LinkMan"/>
	    </set>
```

**在配置`级联`之后，进行`删除`操作时要谨慎。因为可能会`丢失大量数据`。**

### 多对多关系映射：

**用户实体类**

```java
public class SysUser {
	private Long userId;
	private String userCode;
	private String userName;
	private String userPassword;
	private String userState;

	// 多对多关系映射
	private Set<SysRole> roles=new HashSet<SysRole>(0);
    	public Set<SysRole> getRoles() {
		return roles;
	}
	public void setRoles(Set<SysRole> roles) {
		this.roles = roles;
	}

    //get,set方法
}
```

**角色实体类**

```java
public class SysRole {
	private Long roleId;
	private String roleName;
	private String roleMemo;

    //多对多关系映射
	private Set<SysUser> users=new HashSet<SysUser>(0);
	public Set<SysUser> getUsers() {
		return users;
	}
	public void setUsers(Set<SysUser> users) {
		this.users = users;
	}

```

**用户配置文件：**

注：set标签在Class标签中

```xml
 <!-- 多对多关系映射 
	set标签：用于映射集合属性
		属性：
			name：指定集合属性的名称
			table：指定的是中间表的名称（在多对多的配置时，必须写）
	key标签：指定外键字段
		属性：
			column：指定的是当前映射文件所对应的实体在中间表的外键字段名称
	many-to-many标签：指定当前映射文件所对应的实体和集合元素所对应的实体是多对多的关系
		属性：
			class：指定集合元素所对应的实体类
			column：指定的是集合元素所对应的实体在中间表的外键字段名称
	 -->
	 <set name="roles" table="sys_user_role" >
		  <key column="userid"/>
		  <many-to-many class="cn.itcast.domain.SysRole" column="roleid"/>
	 </set>

```

**角色配置文件：**

```xml
 <!-- 多对多关系映射 --> 
<set name="users" table="sys_user_role">
		  <key column="roleid"/>
		  <many-to-many class="cn.itcast.domain.SysUser" column="userid"/>
	 </set>
```

**多对多关系： 保存操作**

```java
@Test
 public void testManyToManySave(){
	//创建两个用户对象
	SysUser u1 = new SysUser();//1号用户
	u1.setUserName("用户1");
	u1.setUserCode("forest@qq.com");
	u1.setUserPassword("123123");
	u1.setUserState("1");
	SysUser u2 = new SysUser();//2号用户
	u2.setUserName("用户2");
	u2.setUserCode("itheima@163.com");
	u2.setUserPassword("123456");
	u2.setUserState("1");
	
	//创建三个角色对象
	SysRole r1 = new SysRole();//1号角色
	r1.setRoleName("角色1");
	r1.setRoleMemo("1级管理员");
	SysRole r2 = new SysRole();//2号角色
	r2.setRoleName("角色2");
	r2.setRoleMemo("超级管理员");
	SysRole r3 = new SysRole();//3号角色
	r3.setRoleName("角色3");
	r3.setRoleMemo("2级管理员");
			
	//建立用户和角色的双向关联关系
	u1.getRoles().add(r1);
	u1.getRoles().add(r2);
	r1.getUsers().add(u1);
	r2.getUsers().add(u1);
			
	u2.getRoles().add(r2);
	u2.getRoles().add(r3);
	r2.getUsers().add(u2);
	r3.getUsers().add(u2);
	
	//从当前线程中获取新的Session对象
	Session  session= HibernateUtil.getCurrentSession();
	Transaction tx =session.beginTransaction();//开启事务
	session.save(u1);
	session.save(u2);
	session.save(r1);
	session.save(r2);
	session.save(r3);
	tx.commit();
 }
}
```

运行结果会报错，因为两张表同时维护一张中间表。

解决：一方放弃外键维护权。操作同一对多。

**`在多对多中禁用双向级联删除`**

### 懒加载：

set标签有一个lazy属性，属性是true表示延迟加载，如果是false就表示立即加载。

如同load方法，使用的是代理对象，在使用时在调用。




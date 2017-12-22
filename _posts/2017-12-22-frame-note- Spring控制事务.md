---
layout: post
title:  "Spring控制事务"
date:   2017-12-22
categories: frame
tags: 事务
---

* content
{:toc}
## 事务:

#### 事务的概念:

   事务是逻辑上一组操作，组成这组操作各个逻辑单元，要么一起成功，要么一起失败。

#### 事务的特性:

- 原子性
-  一致性
-  隔离性
-  持久性

<!-- more -->

#### 如果不考虑隔离性，引发安全问题

- 脏读
- 不可重复读
- 虚读

#### 设置事务隔离级别

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171222/liClLg78l8.png?imageslim)

#### 事务的传播

REQUIRED:如果当前没有事务，就新建一个事务，如果已经存在一个事务中，加入到这个事务中。一般的选择（默认值）

REQUERS_NEW:新建事务，如果当前在事务中，把当前事务挂起。

SUPPORTS:支持当前事务，如果当前没有事务，就以非事务方式执行（没有事务）

MANDATORY：使用当前的事务，如果当前没有事务，就抛出异常

NOT_SUPPORTED:以非事务方式执行操作，如果当前存在事务，就把当前事务挂起

NEVER:以非事务方式运行，如果当前存在事务，抛出异常

NESTED:如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则执行REQUIRED类似的操作。

#### 事务是否只读

建议查询时设置为只读。

## Spring事务管理

`PlatformTransactionManager`此接口是spring的平台事务管理器，它里面提供了我们常用的操作事务的方法。

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171222/h63E66G6ia.png?imageslim)

 我们在开发中都是使用它的实现类

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171222/E1E9gkb6iL.png?imageslim)

## 基于xml的声明式事务控制

```java
public class Account implements Serializable{
	
	private Long id;
	private String name;
	private Double money;
    
    //set,get方法
}
```

```java
public class AccountRowMapper implements RowMapper<Account>{

	/**
	 * 如何把账户表里的一行数据形成账户对象
	 */
	@Override
	public Account mapRow(ResultSet rs, int row) throws SQLException {
		Account account = new Account();
		account.setId(rs.getLong("id"));
		account.setName(rs.getString("name"));
		account.setMoney(rs.getDouble("money"));
		return account;
	}

}
```

```java
public interface AccountDao {
	/**
	 * 根据id查询账户对象
	 * @param id
	 * @return
	 */
	public Account findById(Long id);
	
	/**
	 * 更新账户对象
	 * @param account
	 */
	public void update(Account account);
}
```

```java
public class AccountDaoImpl extends JdbcDaoSupport implements AccountDao {
	
	@Override
	public void update(Account account) {
		this.getJdbcTemplate().update("update account set name = ?,money = ? where id = ?",account.getName(),account.getMoney(),account.getId());
	}


	@Override
	public Account findById(Long id) {
		return this.getJdbcTemplate().queryForObject("select * from account where id = ?", new AccountRowMapper(), id);
	}

}
```

```java
public interface AccountService {

	/**
	 * 业务层：转账方法
	 * @param fromId 转出账户id
	 * @param toId 转入账户id
	 * @param money 转账金额
	 */
	public void transfer(Long fromId,Long toId,Double money);
	
}
```

```java
public class AccountServiceImpl implements AccountService{
	
	private AccountDao accountDao;
	
	public void setAccountDao(AccountDao accountDao) {
		this.accountDao = accountDao;
	}

	@Override
	public void transfer(Long fromId, Long toId, Double money) {
		//查询转出账户
		Account fromAccount = accountDao.findById(fromId);
		//查询转入账户
		Account toAccount = accountDao.findById(toId);
		//转出账户减钱
		fromAccount.setMoney(fromAccount.getMoney() - money);
		//转入账户加钱
		toAccount.setMoney(toAccount.getMoney() + money);
		//更新转出账户
		accountDao.update(fromAccount);
		int i = 10 / 0;
		//更新转入账户
		accountDao.update(toAccount);
	}
}
```

新建jdbc.properties文件：

```
jdbc.driverClass=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/hibernate_itheima16
jdbc.username=root
jdbc.password=123456
```

applicationContext.xml:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xmlns:tx="http://www.springframework.org/schema/tx"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/tx
        http://www.springframework.org/schema/tx/spring-tx.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd">
        <context:property-placeholder location="classpath:jdbc.properties"/>
   
   	 	<bean id="accountService" class="cn.itcast.service.impl.AccountServiceImpl">
        	<property name="accountDao" ref="accountDao"></property>
        </bean>
         <bean id="accountDao" class="cn.itcast.dao.impl.AccountDaoImpl">
        	<property name="dataSource" ref="dataSource"></property>
        </bean>
        
        <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        	<property name="driverClass" value="${jdbc.driverClass}"></property>
        	<property name="jdbcUrl" value="${jdbc.url}"></property>
        	<property name="user" value="${jdbc.username}"></property>
        	<property name="password" value="${jdbc.password}"></property>
        </bean>
        
        <!-- 配置事务管理器 -->
        <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        	<property name="dataSource" ref="dataSource"></property>
        </bean>
   <!-- 配置事务的属性
  		id:指定advice的id，后边要用到
  		transaction-manager:写的是事务管理器的id
  	 -->
  	<tx:advice id="txAdvice" transaction-manager="transactionManager">
  		<tx:attributes>
  			<!-- find开头的方法加只读事务 ，*表示通配符，匹配任意-->
  			<tx:method name="find*" read-only="true"/>
  			<!-- 其余方法是加可读写的事务 -->
  			<tx:method name="*"/>
  		</tx:attributes>
  	</tx:advice>
    	
    <!-- 配置事务切面 -->
  	<aop:config>
  		<!-- 配置切入点表达式：告诉框架哪些方法要控制事务 -->
  		<aop:pointcut expression="execution(* cn.itcast.service.impl.*.*(..))" id="pt"/>
  		<aop:advisor advice-ref="txAdvice" pointcut-ref="pt"/>
  	</aop:config>
</beans>
```

## 基于注解的声明式事务控制:

applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd
        http://www.springframework.org/schema/tx
        http://www.springframework.org/schema/tx/spring-tx.xsd">
         <!-- 开启spring注解扫描 -->
        <context:component-scan base-package="cn.itcast"></context:component-scan>
       <!-- 开启事务注解的支持 transaction-manager:写事务管理器的id -->      
      <tx:annotation-driven transaction-manager="transactionManager"/>
        <context:property-placeholder location="classpath:jdbc.properties"/>
        <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        	<property name="driverClass" value="${jdbc.driverClass}"></property>
        	<property name="jdbcUrl" value="${jdbc.url}"></property>
        	<property name="user" value="${jdbc.username}"></property>
        	<property name="password" value="${jdbc.password}"></property>
        </bean>
        
        <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
   	 		<property name="dataSource" ref="dataSource"></property>
   	 	</bean>
        
        <!-- 配置事务管理器 -->
        <bean id="transactionManager"     class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        	<property name="dataSource" ref="dataSource"></property>
</bean>
```

**注意：此处不能继承**`JdbcDaoSupport`

```java
@Repository("accountDao")
public class AccountDaoImpl implements AccountDao {
	
	@Autowired
	private JdbcTemplate jdbcTemplate;

	@Override
	public void update(Account account) {
		this.jdbcTemplate.update("update account set name = ?,money = ? where id = ?",account.getName(),account.getMoney(),account.getId());
	}

	@Override
	public Account findById(Long id) {
		return this.jdbcTemplate.queryForObject("select * from account where id = ?", new AccountRowMapper(), id);
	}

}

@Service("accountService")
public class AccountServiceImpl implements AccountService{
	
	@Autowired
	private AccountDao accountDao;
  
	@Override
	public void transfer(Long fromId, Long toId, Double money) {
		//......
	}

}
```

```java
@Service("accountService")
@Transactional//该类中所有的方法都加可读写的事务
public class AccountServiceImpl implements AccountService{
	
	@Autowired
	private AccountDao accountDao;


	@Override
	public void transfer(Long fromId, Long toId, Double money) {
		//......
	}
}
```

```java
@Service("accountService")
@Transactional//该类中所有的方法都加可读写的事务
public class AccountServiceImpl implements AccountService{
	
	@Autowired
	private AccountDao accountDao;


	@Override
	public void transfer(Long fromId, Long toId, Double money) {
		//......
	}



	@Override
	@Transactional(readOnly=true)//只读事务
	public void findById(Long id) {
		Account fromAccount = accountDao.findById(id);
		fromAccount.setMoney(10000D);
		accountDao.update(fromAccount);
	}

}
```

提示：@Transactional注解也可以加在方法上，如果类上很方法上都有@Transactional,则以方法上的为准。
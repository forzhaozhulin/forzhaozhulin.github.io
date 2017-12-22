---
layout: post
title:  " Spring常用注解"
date:   2017-12-22
categories: frame
tags: Spring注解
---

* content
{:toc}
## Spring框架中bean管理的常用注解

@Component注解：把资源让spring来管理。相当于在xml中配置一个bean。一般位于类名上面。

@Controller @Service @Repository：他们三个注解都是针对一个的衍生注解，他们的作用及属性都是一模一样的。他们只不过是提供了更加明确的语义化。

​    @Controller：一般用于表现层的注解。

​    @Service：一般用于业务层的注解。

​    @Repository：一般用于持久层的注解。

<!-- more -->

@Value：注入基本数据类型和String类型数据的

@Autowired：自动按照类型注入。当使用注解注入属性时，set方法可以省略。它只能注入其他bean类型。当有多个类型匹配时，使用要注入的对象变量名称作为bean的id，在spring容器查找，找到了也可以注入成功。找不到就报错。

@Qualifer：在自动按照类型注入的基础之上，再按照Bean的id注入。它在给字段注入时不能独立使用，必须和@Autowire一起使用；但是给方法参数注入时，可以独立使用。

@Scope：指定bean的作用范围



### 要使用bean注解先要在applicationContext.xml中开启注解扫描

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">
      <!-- 开启注解扫描 -->
      <context:component-scan base-package="service"></context:component-scan>
</beans>
```

```java
@Service("customerService")
public class CustomerServiceImpl implements CustomerService {
	@Autowired
	private CustomerDao customerDao;
	@Override
	public List<Customer> findAllCustomer() {
		List<Customer> list = customerDao.findAllCustomer();
		return list;
	}
}
```

```java
@Repository("customerDao")
public class CustomerDaoImpl implements CustomerDao {
	@Autowired
	private QueryRunner queryRunner;
	@Override
	public List<Customer> findAllCustomer() {
		List<Customer> list = null;
		try {
			list = queryRunner.query("select * from cst_customer", new BeanListHandler<Customer>(Customer.class));
		} catch (SQLException e) {
			e.printStackTrace();
		}
		return list;
	}
}
```

Spring框架整合JUnit单元测试:

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:applicationContext.xml")
public class Test2 {

	@Autowired
	private UserDao userDao;
	
	@Test
	public void test1(){
		userDao.save();
	}
}
```


#### Aop相关注解：

配置文件：

```xml
   <!-- 开启aspectj自动代理 -->
   <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
```

```java
/**
 * 切面类
 * 
 */
@Component
@Aspect//表示该类是一个切面类
public class MyAspectAnnotation {

	@Before("execution(* dao.impl.CustomerDaoImpl.save(..))")
	public void checkPrivilege(JoinPoint jp){
		System.out.println("检测权限..." + jp);
	}
	
	@AfterReturning(value="execution(* dao.impl.CustomerDaoImpl.delete(..))",returning="result")
	public void afterReturning(Object result){
		System.out.println("后置通知..." + result);
	}
	
	@Around("execution(* dao.impl.CustomerDaoImpl.update(..))")
	public Object around(ProceedingJoinPoint pjp){
		System.out.println("环绕之前通知...");
		Object result = null;
		try {
			result = pjp.proceed();
		} catch (Throwable e) {
			e.printStackTrace();
		}
		System.out.println("环绕之后通知...");
		return result;
	}
	
	@AfterThrowing(value="execution(* dao.impl.CustomerDaoImpl.list(..))",throwing="ex")
	public void afterThrowing(Exception ex){
		System.out.println("这是异常通知..." + ex.getMessage());
	}
	
	@After("execution(* dao.impl.CustomerDaoImpl.list(..))")
	public void after(){
		System.out.println("这是最终通知...");
	}
}
```


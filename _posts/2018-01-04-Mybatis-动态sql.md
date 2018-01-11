---
layout: post
title:  "动态sql和缓存"
date:    2018-01-04
categories: frame
tags: 动态sql mybatis缓存
---

* content
{:toc}
##动态sql

MyBatis 的强大特性之一便是它的动态 SQL。如果你有使用 JDBC 或其他类似框架的经验，你就能体会到根据不同条件拼接 SQL 语句有多么痛苦。拼接的时候要确保不能忘了必要的空格，还要注意省掉列名列表最后的逗号。利用动态 SQL 这一特性可以彻底摆脱这种痛苦。

<!-- more -->

MyBatis的动态SQL是基于OGNL表达式的，它可以帮助我们方便的在SQL语句中实现某些逻辑。 
MyBatis中用于实现动态SQL的元素主要有：

- if
- where
- set
- choose（when，otherwise）
- trim
- foreach

### 1、if标签

查询男性用户，如果输入了用户名，按用户名模糊查询

```xml
	<select id="queryUserListLikeUserName" resultType="User">
		select * from tb_user where sex=1
		<!-- if:判断
			test：OGNL表达式
		 -->
		<if test="userName!=null and userName.trim()!=''">
			 and user_name like '%' #{userName} '%'
		</if>
	</select>
```

### 2、choose when otherwise

查询男性用户，如果输入了用户名则按照用户名模糊查找，否则如果输入了年龄则按照年龄查找，否则查找用户名为“zhangsan”的用户。

```xml
<select id="queryUserListLikeUserNameOrAge" resultType="User">
	select * from tb_user where sex=1
  		<!-- choose:条件选择
			when：test-判断条件，一旦有一个when成立，后续的when都不再执行
			otherwise：所有的when都不成立时，才会执行
		 -->
  <choose>
    <when test="userName!=null and userName.trim()!=''">
    	and user_name like like '%' #{userName} '%'
    </when>
    <when test="age!=null">
    	and age = #{age}
    </when>
    <othersize> and user_name = 'zhangsan'</othersize>
  </choose>
</select>
```



### 3、where

查询所有用户，如果输入了用户名按照用户名进行模糊查询，如果输入年龄，按照年龄进行查询，如果两者都输入，两个条件都要成立。

```xml
	<select id="queryUserListLikeUserNameAndAge" resultType="User">
		select * from tb_user
		<!-- 
			自动添加where关键字
			有一定的纠错功能：去掉sql语句块之前多余的一个and|or
			通常结合if或者choose使用
		 -->
		<where>
			<if test="userName!=null and userName.trim()!=''">user_name like '%' #{userName} 
            '%'</if>
			<if test="age!=null">and age = #{age}</if>
		</where>
	</select>
```

Where：1.添加where关键字 2.去掉动态sql语句块之前多余的一个and

### 4、set

修改用户信息，如果参数user中的某个属性为null，则不修改。

```xml
<update id="updateUserSelective" >
		UPDATE tb_user
		<!-- 
			set自动添加set关键字
			也有一定的纠错功能：自动去掉sql语句块之后多余的一个逗号
		 -->
		<set>
			<if test="userName!=null and userName.trim()!=''">user_name = #{userName},</if>
			<if test="password!=null and password.trim()!=''">password = #{password},</if>
			<if test="name!=null and name.trim()!=''">name = #{name},</if>
			<if test="age!=null">age = #{age},</if>
			<if test="sex!=null">sex = #{sex}</if>
		</set>
		WHERE
			(id = #{id});
	</update>
```

Set：1.添加set关键字 2.去掉动态sql语句块之后多余的一个逗号

### 5、foreach

根据多个id查询用户信息

```xml
	<select id="queryUserListByIds" resultType="User">
		select * from tb_user where id in 
		<!-- 
			foreach:遍历集合
			collection：接收的集合参数
			item：遍历的集合中的一个元素
			separator:分隔符
			open:以什么开始
			close：以什么结束
		 -->
		<foreach collection="ids" item="id" separator="," open="(" close=")">
			#{id}
		</foreach>
	</select>
```

遍历，collection-接收集合数据，item-遍历集合中的一个元素 separator-分隔符 open-以什么开始 close-以什么结束

## 缓存

执行相同的sql语句和参数，mybatis不进行执行sql，而是从缓存中命中返回。

###  一级缓存

在mybatis中，一级缓存默认是开启的，并且一直无法关闭，作用域：在同一个sqlSession下

测试一级缓存：

```java
	@Test
	public void testCache(){
		User user1 = this.userMapper.queryUserById(1l);
		System.out.println(user1);
		System.out.println("=================第二次查询======================");
		User user2 = this.userMapper.queryUserById(1l);
		System.out.println(user2);
	}
```

在打印的日志中：第一次查询是执行sql语句，第二次查询时直接从缓存中命中，即不再执行sql语句。

#### 跟Spring集成的时候(使用mybatis-spring)

```java
@Repository
public class UserDao extends SqlSessionDaoSupport {
    public User selectUserById(int id) {
        SqlSession session = getSqlSession();
        session.selectOne("dao.userdao.selectUserByID", id);
        // 由于session的实现是SqlSessionTemplate的动态代理实现
        // 它已经在代理类内执行了session.close(),所以无需手动关闭session
        return session.selectOne("dao.userdao.selectUserByID", id);
    }
}
```



同样在一个dao中执行两次同样的sql查询，这里执行了2次sql查询,,看似我们使用了同一个sqlSession,但是实际上因为我们的dao继承了SqlSessionDaoSupport,而SqlSessionDaoSupport内部sqlSession的实现是使用用动态代理实现的,这个动态代理sqlSessionProxy使用一个模板方法封装了select()等操作,每一次select()查询都会自动先执行openSession(),执行完close()以后调用close()方法,相当于生成了一个新的session实例,所以我们无需手动的去关闭这个session()(关于这一点见下面mybatis的官方文档),当然也无法使用mybatis的一级缓存,也就是说mybatis的一级缓存在spring中是没有作用的.i

### 清除缓存：

使用：sqlSession.clearCache();可以强制清除缓存

执行update、insert、delete的时候，会清空缓存

### 二级缓存

二级缓存就是global caching,它超出session范围之外,可以被所有sqlSession共享,它的实现机制和mysql的缓存一样,开启它只需要在mybatis的配置文件开启settings里的

```xml
<setting name="cacheEnabled" value="true"/>
```

以及在相应的Mapper文件里开启:

```xml
<mapper namespace="dao.userdao">
       <!-- Cache 配置 -->
    <cache />
  ...  select statement ...
</mapper>
```



global caching的作用域是针对Mapper的Namespace而言的,也就是说只在有在这个Namespace内的查询才能共享这个cache.

注意：

​	由于缓存数据是在sqlSession调用close方法时，放入二级缓存的，所以第一个sqlSession必须先关闭

​	二级缓存的对象必须序列化，例如：User对象必须实现Serializable接口。
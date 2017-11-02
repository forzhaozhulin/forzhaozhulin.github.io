---
layout: post
title:  "API--常用类"
date:   2017-10-29
categories: Java
tags: Scanner Random System Object Date Calendar
---

* content
{:toc}
## Scanner类：

**Scanner类是一个基于正则表法师的文本扫描器。**

---

主要实现键盘录入；

 键盘录入：  

1. 导包  	
  2. 创建对象  
2. 接收数据

```java
Scanner sc = new Scanner(System.in);
System.out.println("请录入一个整数：");
int i = sc.nextInt();
```

不同类型对应不同方法 若是long型则用nextLong()

字符串用nextLine()。

Scanner主要提供两个方法来扫描输入：   

* hasNextXxx()：是否还有下一个输入项；    
* nextXxx()：获取下一个输入项；



**Scanner还能读取文件输入只要创建对象时传入File对象**

```java
Scanner sc = new Scanner (new File("a.txt") );
while(sc.hasNextLine()){
      System.out.println(sc.nextLine());
}
```

## Random类：

Random类用于生成随机数.

使用步骤(和Scanner类似)

* 导包 import java.util.Random;

* 创建对象 Random r = new Random();

* 获取随机数 int number = r.nextInt(10);

  ​	产生的数据在0到10之间，包括0，不包括10。

  ​	括号里面的10是可以变化的，如果是100，就是0-100之间的数据

  ​	r.nextInt(b-a+1)+a;产生a到b之间的随机数（包括a和b）

## System类：

**System类代表当前Java程序的运行平台，不能创建System类的对象。**

**方法：**

```
static void arraycopy(Object src, int srcPos, Object dest, int destPos, int length) :
	从src源数组的srcPos索引开始,复制length个元素
		从destPost位置开始将这些元素放至到dest数组中
static long currentTimeMillis() 
 	返回以毫秒为单位的当前时间
static void exit(int status) 
	终止当前正在运行的 Java 虚拟机
```

**以毫秒值返回当前系统时间（1970-1-1 0:0:0至今过了多少毫秒）**

**1000毫秒 = 1秒**

## Object类:

**Object类是Java语言中的`根类`，即所有类的父类。它中描述的所有方法子类都可以使用。所有类在创建对象的时候，最终找的父类就是Object。**

**当定义一个类时没有使用extends关键字为它显式指定父类，则该类默认继承Object父类**

```
 boolean equals(Object obj)
  	 判断是否相等
final Class<?> getClass()
	返回此 Object 的运行时类，即 Object的字节码对象
String toString()
	返回该对象的字符串表示
```

## Date类：

Java提供了Date类来处理日期、时间。

Date类从JDK1.0起就开始存在了。但正因为它历史悠久，所以它的大部分构造器、方法都已经过时，不再推荐使用了。

### 构造方法：

```
Date() ：创建的是一个表示当前系统时间的Date对象
Date(long date) ：根据"指定时间"创建Date对象
```

Date底层操作是System.currentTimeMillis()。

常用方法：

```
void setTime(long time)  
long getTime()
```

```java
import java.util.Date;
public class DateDemo {
	public static void main(String[] args) {
		Date d = new Date(1000);
		System.out.println(d);
	}
}
```

输出：

Thu Jan 01 08:00:01 CST 1970

## DateFormat类 & SimpleDateFormat：

 DateFormat 是日期/时间格式化子类的抽象类，它以与语言无关的方式格式化并解析日期或时间。日期/时间格式化子类（如 SimpleDateFormat类）允许进行格式化（也就是日期 -> 文本）、解析（文本-> 日期）和标准化。

这个类可以完成日期和文本之间的转换。

DateFormat是抽象类，我们需要使用其子类SimpleDateFormat来创建对象。

###  **SimpleDateFormat**构造方法；

```
SimpleDateFormat(String pattern)
用给定的模式和默认语言环境的日期格式符号构造 SimpleDateFormat
```

### DateFormat类方法：

```
String format(Date date)
将一个 Date 格式化为日期/时间字符串。
Date parse(String source)
从给定字符串的开始解析文本，以生成一个日期。
```

```java
public class SimpleDateFormatDemo {
	public static void main(String[] args) throws ParseException {
		//4个小姨2个大美眉和2个小弟弟
		SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss");
		
		//格式化
		Date date = new Date();
		String s = sdf.format(date);
		System.out.println(s);//2049年08月26日 13:39:12
	}
```

## Calendar类

Calendar是日历类，在Date后出现，替换掉了许多Date的方法。该类将所有可能用到的时间信息封装为静态成员变量，方便获取。

Calendar为抽象类，由于语言敏感性，Calendar类在创建对象时并非直接创建，而是通过静态方法创建，将语言敏感内容处理好，再返回子类对象

Calendar c = Calendar.getInstance();  //返回当前时间

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171101/0gB6LhlFbL.png?imageslim)

```java
public class CalendarDemo {
	public static void main(String[] args) {
		//static Calendar getInstance()  
		Calendar c = Calendar.getInstance();
		
		c.add(Calendar.DAY_OF_MONTH, -1);
		
		int year = c.get(Calendar.YEAR);
		int month = c.get(Calendar.MONTH) + 1;
		int day = c.get(Calendar.DAY_OF_MONTH);
		
		System.out.println(year + "年" + month + "月" + day + "日");
		 
	}
}
```
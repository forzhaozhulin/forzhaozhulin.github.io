---
layout: post
title:  "流程控制(循环结构)"
date:   2017-10-31
categories: Java
tags: if for循环 while循环
---

* content
{:toc}
## 顺序结构：

是程序中最简单最`基本`的流程控制，没有特定的语法结构，按照代码的`先后顺序`，依次执行，程序中大多数的代码都是这样执行的。（`从上往下`，依次执行）

---

## 选择流程控制：

### If语句：

if语句有三种格式

1.

```
if(关系表达式) {

    语句体;
    
}
```

判断关系表达式看其结果是true还是false

如果是true就执行语句体

2.

```
if(关系表达式) {

     语句体1;

}else {

     语句体2;

}	
```

首先判断关系表达式看其结果是true还是false

如果是true就执行语句体1

如果是false就执行语句体2

3.

```
if(关系表达式1) {

     语句体1;

}else  if (关系表达式2) {

     语句体2;

}

    …

else {

     语句体n+1;

}
```

首先判断关系表达式1看其结果是true还是false

如果是true就执行语句体1

如果是false就继续判断关系表达式2看其结果是true还是false

如果是true就执行语句体2

如果是false就继续判断关系表达式…看其结果是true还是false

…

如果没有任何关系表达式为true，就执行语句体n+1

**if小结：**

if语句的三种格式：

 	第一种格式适合做一种情况的判断

  	第二种格式适合做二种情况的判断

 	第三种格式适合做多种情况的判断

### switch语句:

```
switch语句格式：
  switch(表达式) {
 	case 值1:
 	语句体1;
 	break;
 	case 值2:
  	语句体2;
  break;
 	...
  default:
 	语句体n+1;
 	break;
 }
```

switch表示这是switch语句

* 表达式的取值：byte,short,int,char
* JDK5以后可以是枚举
* JDK7以后可以是String
* case后面跟的是要和表达式进行比较的值
* 语句体部分可以是一条或多条语句
* break表示中断，结束的意思，可以结束switch语句
* default语句表示所有情况都不匹配的时候，就执行该处的内容，和if语句的else相似。

---

## 循环流程控制

### for循环:

```
for(初始化语句;判断条件语句;控制条件语句) {
         循环体语句;
    }
```

#### 执行流程

1. 执行初始化语句

2. 执行判断条件语句，看其结果是true还是false

   如果是false，循环结束。

   如果是true，继续执行。

3. 执行循环体语句

#### for循环实现1-100的和

```java
public class GetSum {
	public static void main(String[] args) {
		//定义求和变量，初始化值是0
		int sum = 0;
		
		//获取1-100之间的数据，用for循环实现
		for(int x=1; x<=100; x++) {
				sum += x;
		}
		System.out.println("1-100的和是:"+sum);
	}
}
```

### while循环：

```
基本格式
   while(判断条件语句) {
         循环体语句;
   }
扩展格式
   初始化语句;
   while(判断条件语句) {
         循环体语句;
         控制条件语句;
}
```

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171031/j5icgD9im5.jpg?imageslim)



```java
public class WhileGetSum {
	public static void main(String[] args) {
		
		//while循环实现
		//定义求和变量
		int sum = 0;
		int x = 1;
		while(x<=100) {
			sum += x;
			x++;
		}
		System.out.println("1-100的和是："+sum);
	}
}
```

### do…while循环

```
基本格式
   do {
         循环体语句;
   }while((判断条件语句);
扩展格式
   初始化语句;
   do {
         循环体语句;
         控制条件语句;
} while((判断条件语句);
```

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171031/eajj0G6dDC.jpg?imageslim)

### 三种循环的区别:

1. do…while循环至少会执行一次循环体。

2. for循环和while循环只有在条件成立的时候才会去执行循环体

3. for循环语句和while循环语句的小区别：

   控制条件语句所控制的那个变量，在for循环结束后，就不能再被访问到了，而while循环结束还可以继续使用，如果你想继续使用，就用while，否则推荐使用for。原因是for循环结束，该变量就从内存中消失，能够提高内存的使用效率

## 控制循环语句:

#### 控制跳转语句break

break的使用场景：

​	在选择结构switch语句中

​	在循环语句中

离开使用场景的存在是没有意义的

​	`break的作用`：

​	跳出单层循环

#### 控制跳转语句continue

continue的使用场景：      

在循环语句中

`continue的作用`：

跳出单层循环对比break，然后总结两个的区别

1. break  退出当前循环
2. continue  退出本次循环
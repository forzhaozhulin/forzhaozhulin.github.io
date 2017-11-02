---
layout: post
title:  "String"
date:   2017-11-01
categories: Java
tags: String
---

* content
{:toc}
学习API就是学习一个个的类，学习类先看他的继承关系，看看我们有没有学过他的子父类，子父类都有一些共性，一些相同的方法，然后看这个类的构造方法，有了构造方法就可以创建对象，调用方法。

---

# String：

- `String` 类代表字符串。字符串是常量；它们的值在创建之后不能更改
- 因为 String 对象是不可变的，所以可以共享。
- 多个字符组成的一串数据。其实它可以和字符数组进行相互转换

### 构造方法（部分）：

```
String(String original):把字符串数据封装成字符串对象
String(char[] value):把字符数组的数据封装成字符串对象
String(char[] value, int index, int count):把字符数组中的一部分数据封装成字符串对象 
String(byte[] value):把字节数组的数据封装成字符串对象
String(byte[] value, int index, int count):把字节数组中的一部分数据封装成字符串对象  
String s = "hello"; 这一个虽然不是构造方法，但是结果也是一个字符串对象
```

### 方法（部分）：

1. 判断功能：

   ```
      boolean equals(Object obj):比较字符串的内容是否相同
      boolean equalsIgnoreCase(String str):比较字符串的内容是否相同,忽略大小写
      boolean startsWith(String str):判断字符串对象是否以指定的str开头
      boolean endsWith(String str):判断字符串对象是否以指定的str结尾
   ```

2. 获取功能：

   ```
      int length():获取字符串的长度，其实也就是字符个数
      char charAt(int index):获取指定索引处的字符
      int indexOf(String str):获取str在字符串对象中第一次出现的索引
      int  lastIndexof(String str):获取str在字符串对象中最后一次出现的索引
      String substring(int start):从start开始截取字符串
      String substring(int start,int end):从start开始，到end结束截取字符串。包括start，不包括end
   ```

3. 转换功能：

   ```
       byte[] getBytes()：把字符串转换为字节数组
   	char[] toCharArray()：把字符串转换为字符数组
   	static String valueOf(char[] chs)：把字符数组转换为字符串
   	static String valueOf(int i)：把整数转换为字符串
   	String toLowerCase()：把字符串转换为小写字符串
   	String toUpperCase()：把字符串转换为大写字符串
   	String concat(String str)：把字符串（str）连接到字符串的结尾 （一般不用） 
   ```

4. 其他：

   ```
       String trim()：去除字符串两端空格
       String[] split(String str)：按照指定符号分割字符串
   ```

# StringBuilder

用字符串做拼接，比较耗时并且也耗内存,Java就提供了 两个字符串缓冲区类。StringBuilder和StringBuffered。

#### String和StringBuilder的区别：

* String的内容是固定的


* StringBuilder的内容是可变的

#### StringBuffered和StringBuilder的区别

* StringBuffer：同步的，数据安全，效率低。
* StringBuilder：不同步的，数据不安全，效率高。

我们一般使用StringBuilder；

#### **StringBuilder的构造方法：**

```java
StringBuilder sb = new StringBuilder();
StringBuilder sb = new StringBuilder(String str);
```

#### StringBuilder的方法：

```
    public int capacity():返回当前容量 (理论值)
    public int length():返回长度(已经存储的字符个数)
	public StringBuilder append(任意类型):添加数据，并返回自身对象
	public StringBuilder reverse():反转功能
```

#### **StringBuilder和String的相互转换**

```
	StringBuilder -- String
  	public String toString():通过toString()就可以实现把StringBuilder转成String
	StringBuilder(String str):通过构造方法就可以实现把String转成StringBuilder
```




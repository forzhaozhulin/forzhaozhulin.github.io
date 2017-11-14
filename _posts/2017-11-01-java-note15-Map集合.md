---
layout: post
title:  "Map集合"
date:   2017-11-01
categories: Java
tags: Map TreeMap Properties Map遍历
---

* content
{:toc}
{:toc}

[TOC]

# Map

Map用于保存具有映射关系的数据，Map中的集合，元素是成对存在的(理解为夫妻)。

<!-- more -->

---

每个元素由键与值两部分组成，通过键可以找对所对应的值。

Collection中的集合称为单列集合，Map中的集合称为双列集合。

Map中的集合不能包含重复的键，值可以重复；每个键只能对应一个值。

所以Map的所有键就是一个Set集合。

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171101/h0F1c6Dc9l.jpg?imageslim)

## Map常用功能:

1.映射功能：

```
V put(K key, V value) :以键=值的方式存入Map集合
```

2.获取功能：

```
V get(Object key):根据键获取值
int size():返回Map中键值对的个数
```

3.判断功能：

```
 boolean containsKey(Object key):判断Map集合中是否包含键为key的键值对
 boolean containsValue(Object value):判断Map集合中是否包含值为value键值对
 boolean isEmpty():判断Map集合中是否没有任何键值对 
```

4.删除功能：

```
 void clear():清空Map集合中所有的键值对
 V remove(Object key):根据键值删除Map中键值对
```

5.遍历功能

```
Set<Map.Entry<K,V>> entrySet():将每个键值对封装到一个个Entry对象中,再把所有Entry的对象封装到Set集合中返回
Set<K> keySet() :将Map中所有的键装到Set集合中返回
Collection<V> values():返回集合中所有的value的值的集合
Set<K> keySet() : 以Set的形式获返回所有的key
Collection<V> values() : 以Collection的形式返回所有的values
```

## Map的遍历：

### 利用keySet()方法遍历：

```java
public class MapTest {
	public static void main(String[] args) {
		//创建Map对象
		Map<String,String> map = new HashMap<String,String>();
		//添加映射关系
		map.put("001", "张箔纸");
		map.put("002", "钟欣桶");
		map.put("003", "王飞");
		//遍历Map对象
		
		//获得所有key
		Set<String> keys = map.keySet();
		//遍历key
		for (String key : keys) {
			//利用key得到每一值
			String value = map.get(key);
			System.out.println( key + "---" " + value);
		}
		
	}
}
```

### 利用entrySet()方法遍历

```java
public class MapDemo5 {
	public static void main(String[] args) {
		//创建Map对象
		Map<String,String> map = new HashMap<String,String>();
		//添加映射关系
		map.put("10010", "小龙女");
		map.put("10086", "东方菇凉");
		map.put("10000", "叶二娘");
		//获取所有的映射关系
		Set<Map.Entry<String,String>> entrys = map.entrySet();
		//遍历包含了映射关系对象的集合
		for (Map.Entry<String, String> entry : entrys) {
			//获取每个单独的映射关系
			//通过映射关系对象获取key和value
			String key = entry.getKey();
			String value = entry.getValue();
			System.out.println(" key + "---" + value);
		}
		
	}
}
```

---

### HashMap和Hashtable：

两者都是Map接口的典型实现类，完全类似ArrayList 和vector

Hashtable从类名上可以看出他是一个古老的类。”t“应该大写的

1. Hashtable线程安全，HashMap线程不安全。所以HashMap性能高一点；

   但如果有多个线程同时访问一个Map对象，应使用Hashtable

2. Hashtable不允许null作为key和value，但HashMap可以。



### Hashtable的实现类Properties：

该类对象在处理属性文件时特别方便。

`Properties`可以把Map对象和属性文件关联起来从而把key-value写入文件

也可以把文件中的“属性名=属性值”加载到Map对象中

该集合`没有泛型`。键值都是`字符串`。

方法：

```
String getProperty(String key)
用指定的键在此属性列表中搜索属性。
String getProperty(String key, String defaultValue)
用指定的键在属性列表中搜索属性。如果未找到属性，则此方法返回默认值变量。
Object setProperty(String key, String value)
设置属性值

void load(InputStream inStream)
从输入流中读取属性列表（键和元素对
void list(PrintStream out)
将属性列表输出到指定的输出流
void store(OutputStream out,String comments)
以适合使用load(InputStream)方法加载到Properties表中的格式，将此Properties表中的属性列表（键和元素对）写入输出流。

```

### TreeMap实现类：

红黑树数据结构每个key-value作为一个节点

和TreeSet一样会对节点进行排序；

自然排序or定制排序（传入Comparator对象）




























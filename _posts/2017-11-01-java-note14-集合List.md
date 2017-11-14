---
layout: post
title:  "集合List"
date:   2017-11-01
categories: Java
tags: List Queue 数据结构 增强for
---

* content
{:toc}
## 常见数据结构：

1.数组，采用该结构的集合，对元素的存取有如下的特点：

* 查找元素快：通过索引，可以快速访问指定位置的元素


* 增删元素慢 ,每次添加元素需要移动大量元素或这创建新的数组

<!-- more -->

2.链表，采用该结构的集合，对元素的存取有如下的特点：

* 多个节点之间，通过地址进行连接。例如，多个人手拉手，每个人使用自己的右手拉住下个人的左手，依次类推，这样多个人就连在一起了。
* 查找元素慢：想查找某个元素，需要通过连接的节点，依次向后查找指定元素
* 增删元素快：

3.堆栈，采用该结构的集合，对元素的存取有如下的特点：

​	先进后出（即，存进去的元素，要在后它后面的元素依次取出后，才能取出该元素）。

​	例如，子弹压进弹夹，先压进去的子弹在下面，后压进去的子弹在上面，当开枪时，先弹出上面的子弹，然后才能弹出下面的子弹。

4.队列，采用该结构的集合，对元素的存取有如下的特点：

​	先进先出（即，存进去的元素，要在后它前面的元素依次取出后，才能取出该元素）。

​	例如，安检。排成一列，每个人依次检查，只有前面的人全部检查完毕后，才能排到当前的人进行检查。

## 增强for

增强for循环是JDK1.5以后出来的一个高级for循环，专门用来遍历数组和集合的。

它的内部原理其实是个Iterator迭代器，所以在遍历的过程中，不能对集合中的元素进行增删操作。

格式：

```
for(元素的数据类型  变量 : Collection集合or数组){
}
```

```java
public class ForEachDemo {
	public static void main(String[] args) {
		 //创建集合对象
		Collection<String> c = new ArrayList<String>();
		//添加元素
		c.add("hello");
		c.add("world");
		c.add("java");
		
		
		for (String string : c) {
			System.out.println(string);
		}
	}
}
```

## List

### List子体系特点：

1. 有序的（存储和读取的顺序是一致的） 
2. 有整数索引 
3. 允许重复的

### List的特有功能：

```
void add(int index, E element) :将元素添加到index索引位置上
E get(int index) :根据index索引获取元素
E remove(int index) :根据index索引删除元素
E set(int index, E element):将index索引位置的的元素设置为element
```

List还提供一个listIterator，返回一个ListIterator对象，该接口继承Iterator。

增加的方法：

```
boolean hasPrevious()：返回集合是否还有上一个元素
Object previous()：返回该迭代器的上一个元素，
void add() 在指定位置插入一个元素
```

 List和Set一样是接口，`依赖`于具体实现类。

List的实现类有：`ArrayList`、`LinkedList`、vector



### ArrayList和vector：

ArrayList和vector都是基于数组的List类，内封装一个动态的允许再分配的数组。
默认长度为10.长度不够时，增加一半长度

ArrayList和vector，两者用法基本相同，但vector是一个古老的集合（jdk1.0开始就有）

在jdk1.2后才改为实现List接口所以vector有一些方法功能重复，

vector还有很多缺点， 不推荐使用。

ArrayList和vector区别：

* ArrayList线程不安全。效率高
* vector线程安全，效率低，不过即使要求线程安全也不用他

### LinkedList特有功能

```
 void addFirst(E e) :向链表的头部添加元素
 void addLast(E e):向链表的尾部添加元素
 E getFirst():获取链头的元素,不删除元素
 E getLast():获取链尾的元素,不删除元素
 E removeFirst():返回链头的元素并删除链头的元素
 E removeLast():返回链尾的元素并删除链尾的元素
```

List的常用子类：

* ArrayList

   底层是数组结构，查询快，增删慢

* LinkedList

  底层结构是链表，查询慢，增删快

 如何选择使用不同的集合？

1. 如果查询多，增删少，则使用ArrayList
2. 如果查询少，增删多，则使用LinkedList
3. 如果你不知道使用什么，则使用ArrayList

## Queue集合：

Queue用于模拟队列，采用“先进先出”(FIFO)

通常队列不允许随机访问队列中的元素。

方法：

```
boolean add(E e)
将指定的元素插入此队列尾部
E element()
获取，但是不移除此队列的头。此方法与 peek 唯一的不同在于：此队列为空时将抛出一个异常。
boolean offer(E e)
将指定的元素插入此队列（如果立即可行且不会违反容量限制），当使用有容量限制的队列时，此方法通常要优于 add(E)，
E peek()
获取但不移除此队列的头；如果此队列为空，则返回 null。
E poll()
获取并移除此队列的头，如果此队列为空，则返回 null。
E remove()
获取并移除此队列的头。此方法与 poll 唯一的不同在于：此队列为空时将抛出一个异常。

```

Queue接口有一个实现类PriorityQueue和一个子接口Deque

Deque代表舒安端队列，可以同时在两端来添加、删除元素

Deque·提供一个典型实现类：ArrayDeque。

方法：

```
E getFirst()
获取，但不移除此双端队列的第一个元素。

E getLast()
获取，但不移除此双端队列的最后一个元素
boolean offer(E e)
将指定元素插入此双端队列所表示的队列（换句话说，此双端队列的尾部），
如果可以直接这样做而不违反容量限制的话；如果成功，则返回 true，
如果当前没有可用的空间，则返回false
boolean offerLast(E e)
在不违反容量限制的情况下，将指定的元素插入此双端队列的末尾
boolean offerFirst(E e)
在不违反容量限制的情况下，将指定的元素插入此双端队列的开头
E pollLast()
获取并移除此双端队列的最后一个元素；如果此双端队列为空，则返回 null。
E pollFirst()
获取并移除此双端队列的第一个元素；如果此双端队列为空，则返回 null。
E remove()
获取并移除此双端队列所表示的队列的头部（换句话说，此双端队列的第一个元素
E pollFirst()
获取并移除此双端队列的第一个元素；如果此双端队列为空，则返回 null。
E pollLast()
获取并移除此双端队列的最后一个元素；如果此双端队列为空，则返回 null。

```




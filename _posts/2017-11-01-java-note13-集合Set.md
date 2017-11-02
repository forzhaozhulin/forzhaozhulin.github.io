---
layout: post
title:  "集合Set"
date:   2017-11-01
categories: Java
tags: 集合 Set
---

* content
{:toc}
# 集合：

为了保存数量不确定的数据，以及保存具有映射关系的数据（也被称为关联数组）。Java提供集合类，集合类主要负责保存、盛装其他数据，因此集合类也被称为容器类。

---

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171101/1ib9E0mhfH.png?imageslim)

在最顶层的父接口Collection中一定定义了所有子类集合的共同属性和方法,因此我们首先需要学习Collection中共性方法,然后再去针对每个子类集合学习它的特有方法

集合的体系结构：

 	由于不同的数据结构（数据的组织，存储方式），所以Java为我们提供了不同的集合，但是不同的集合他们的功能都是相似，不断的向上提取，将共性抽取出来，这就是集合体系结构形成的原因。

### Collection中的常用方法：

```
boolean add(Object e): 向集合中添加元素
void clear():清空集合中所有元素
boolean contains(Object o):判断集合中是否包含某个元素
boolean isEmpty():判断集合中的元素是否为空
boolean remove(Object o):根据元素的内容来删除某个元素
int size():获取集合的长度
Object[] toArray():能够将集合转换成数组并把集合中的元素存储到数组中
```

### 迭代器：

Java中提供了很多个集合，它们在存储元素时，采用的存储方式不同。我们要取出这些集合中的元素，可通过一种通用的获取方式来完成。

Collection集合元素的通用获取方式：在取元素之前先要判断集合中有没有元素，如果有，就把这个元素取出来，继续在判断，如果还有就再取出出来。一直把集合中的所有元素全部取出。这种取出方式专业术语称为迭代。

集合中把这种取元素的方式描述在Iterator接口中。Iterator接口的常用方法如下    

```
hasNext（）方法：判断集合中是否有元素可以迭代        
next（）方法：用来返回迭代的下一个元素，并把指针向后移动一位。
```

```java
public class IteratorTest {
	public static void main(String[] args) {
		//创建集合对象
		Collection c = new ArrayList();
		//添加元素
		c.add("hello");
		c.add("world");
		c.add("java");
		
		//获取迭代器对象
		Iterator it = c.iterator();
		
		//Object next()  :返回下一个元素
		//boolean hasNext()  ：判断是否有元素可以获取
		
		
		while(it.hasNext()) {
			System.out.println(it.next());
		}
	}
}
```

并发修改异常产生原因:      当使用迭代器遍历集合的时候,使用了集合中的 增加/删除 方法,导致并发修改异常产

解决方案:   

1. 不使用迭代器遍历集合,就可以在遍历的时候使用集合的方法进行增加或删除  
2. 依然使用迭代器遍历,那么就需要使用Iterator的子接口ListIterator来实现向集合中添加

# Set：

### Set集合的特点:

- 存入集合的顺序和取出集合的顺序不一致
- 没有索引
- 存入集合的元素没有重复

set集合与collection 基本相同。没有提供额外的方法，实际上set就是collection，只是行为的不同（不允许重复）

如果试图添加两个相同的元素，则add失败，返回FALSE，新元素不会被加

Set是接口，不能创建对象。只能依赖实现类。主要是HashSet和TreeSet。

### **唯一性原理：**

新添加到HashSet集合的元素都会与集合中已有的元素一一比较        

​	首先比较哈希值(每个元素都会调用hashCode()产生一个哈希值)            

​		 如果新添加的元素与集合中已有的元素的哈希值都不同,新添加的元素存入集合            

​		 如果新添加的元素与集合中已有的某个元素哈希值相同,此时还需要调用equals(Object obj)比较                   

​			如果equals(Object obj)方法返回true,说明新添加的元素与集合中已有的某个元素的属性值相同,那		么新添加的元素不存入集合                

​		  	 如果equals(Object obj)方法返回false, 说明新添加的元素与集合中已有的元素的属性值都不同, 那么新添加的元素存入集合

### HashSet

`HashSet`是Set接口的`典型实现`，大多数时候都是使用它，具有不错的存取和查找性能。

HashSet不是同步的，不安全的。

**他的集合元素解一时null注意：**

**当一个对象放入HashSet中时；应重写对象对应类的equals方法，和hashcode方法**

### TreeSet：

TreeSet会调用集合元素的compareTo（Object ob）方法来比较元素之间的大小关系，然后按升序排列这就是自然排序；

如果要实现定制排序。则需要在创建TreeSet集合对象时，提供一个comparator对象与该TreeSet集合关联，由comparator对象负责集合元素的排序逻辑。

TreeSet可以保持集合元素的排序状态。

TreeSet还提供额外的方法：

```
Object first()：返回集合的第一个元素；
Object last(）: 返回集合的最后一个元素；
Object lower(Object e ) 
返回此 set 中严格小于给定元素的最大元素；如果不存在这样的元素，则返回 null。
object higher(Object e)
返回此 set 中严格大于给定元素的最小元素；如果不存在这样的元素，则返回 null。

SortedSetObject subSet(Object fromElement,Oject toElement)
返回此 set 的子集合，其元素从 fromElement（包括）到 toElement（不包括）。

 SortedSet<E> headSet(E toElement)
返回此 set 的子集合，其元素严格小于 toElement。
SortedSet<E> tailSet(E fromElement)
返回此 set 的子集合，其元素大于等于 fromElement
```

HashSet和TreeSet如何选择呢，只有需要一个保持排序的Set时才用TreeSet，否则都应该使用HashSet

Set的三个实现类都是不安全的，若有多个线程同时访问Set集合，必须手动保证Set集合的同步性。

使用Collections工具类中的synchronizedSortedSet方法来“包装”该Set集合
该操作可以在创建时完成。


---
layout: post
title:  "数据类型和运算符"
date:   2017-10-29
categories: Java
tags: 
---

* content
{:toc}
# xml:

xml语言是具有结构性的`标记语言`,  可以灵活的存储一对多的数据关系.之前学习过`properties`配置文件, 通过这种配置文件, 可以使代码的编写更加灵活.但是这种配置文件也只能存储一个`键值对`的`映射`关系, 如果需要存储多个没错, 可以使用xml ,用来当做**`配置文件`**存储数据。

xml文件:存储数据,以标签的形式存储(     &lt;name&gt;Jack&lt;/name&gt;      ).xml文件:存储数据,以标签的形式存储(&lt;name&gt;Jack&lt;/name&gt;).

 xml中的元素其实就是一个个的`标签`

### 标签分为两种：

1.包含标签体；

```xml
    <student>
                        <name>zhangsan</name>
                        <age>18</age>
                    </student>
```

2.不包含标签体：

```xml
	<student 
                        name="zhangsan"
                        age="18"
                    />
```

### 元素的书写规范：

 严格区分大小写；&lt;p&gt;  &lt;P&gt;

只能以字母或下划线开头；abc  _abc

不能以xml(或XML、Xml等)开头----W3C保留日后使用；    

名称字符之间不能有空格或制表符；    

名称字符之间不能使用冒号 : (有特殊用途)

#### 元素中属性的注意事项：

 一个元素可以有多个属性，每个属性都有它自己的名称和取值。
​            属性值一定要用引号(单引号或双引号)引起来。
​            属性名称的命名规范与元素的命名规范相同
​            元素中的属性是不允许重复的

在XML技术中，标签属性所代表的信息也可以被改成用子元素的形式来描述

```xml
<?xml version="1.0" encoding="UTF-8"?>
<students>
	<student name="zhangsan" age="18" />
	
	
	<student>
		<name>zhangsan</name>
		<age>18</age>
	</student>
</students>
```

### xml的注释：

&lt;!-- 被注释的内容 --&gt;

注意: 注释不能嵌套定义

#### CDATA区：

CDATA区中表示全是文本,把标签当做普通文本内容

```xml
<students>
	<student>
		<name>zhangsan</name>
		<url>
			<![CDATA[
				<Stu></Stu>
                <Stu></Stu>
                <Stu></Stu>
			]]>
		</url>
	</student>
```

**对于一些特殊字符，若要在元素主体内容中显示，必须进行`转义`**

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171107/G8HiD09C33.png?imageslim)



## 约束

XML技术中，可以编写一个文档来约束一个XML的书写规范，这个文档称之为约束.约束文档定义了在XML中允许出现的标签名称、属性及标签出现的顺序等等

约束方式：DTD ,  Schema .

- XML Schema符合XML语法结构。 
- DOM、SAX等XML API很容易解析出XML Schema文档中的内容。 
- XML Schema对名称空间支持得非常好。 
- XML Schema比XML DTD支持更多的数据类型，并支持用户自定义新的数据类型。 
- XML Schema定义约束的能力非常强大，可以对XML实例文档作出细致的语义限制。

# Java解析XML

XML解析方式分为两种：DOM方式和SAX方式

- DOM：Document Object Model，文档对象模型。这种方式是W3C推荐的处理XML的一种方式。
- SAX：Simple API for XML。这种方式不是官方标准，属于开源社区XML-DEV，几乎所有的XML解析器都支持它

XML解析开发包:

​         JAXP：是SUN公司推出的解析标准实现。

​        Dom4J：是开源组织推出的解析开发包。(牛，大家都在用，包括SUN公司的一些技术的实现都在用。)

DOM: 将整棵树一口气全部加载到内存当中, 我们可以非常方便的操作任意的标签和属性.

但是, 如果整棵树特别大的时候, 会出现内存溢出的问题

SAX: 一个节点一个节点的进行解析

节点: 标签、属性、文本、甚至是换行都称之为节点

```java
 Dom4J的常用方法：
 	Document
 		Element getRootElement() 
        获取根元素对象（根标签）
 	Element
 		List elements() ：
        获取所有的子元素
 		List elements(String name)：
        根据指定的元素名称来获取相应的所有的子元素
 		Element element(String name)
        根据指定的元素名称来获取子元素对象,如果元素名称重复，则获取第一个元素 
 		String	elementText(String name) ：
        根据指定的子元素名称，来获取子元素中的文本
 		String	getText() ：
        获取当前元素对象的文本
  		void setText(String text)：
        设置当前元素对象的文本
  		String	attributeValue(String name)：
        根据指定的属性名称获取其对应的值
  		public Element addAttribute(String name,String value)
        根据指定的属性名称和值进行添加或者修改
        
```

```xml
<?xml version="1.0" encoding="UTF-8"?>

<State Code="37" Name="河南" description="郑州" GDP="9999亿" 人口="999亿"> 
  <City> 
    <Name>郑州</Name>  
    <Region>高薪区</Region> 
  </City>  
  <City>北京</City>  
  <City>龙门</City>  
  <City>三门峡</City>  
  <City>安阳</City>  
 
</State>

```

```java
public class Dom4JUtils {
	private Dom4JUtils(){}
	
	public static Document getDocument() throws Exception{
		SAXReader reader = new SAXReader();
		Document document = reader.read("src/city.xml");
		return document;
	}

	public static void Writexml(Document document) throws IOException {
		OutputFormat format = OutputFormat.createPrettyPrint();
		//format.setEncoding("UTF-8");//默认的编码就是UTF-8
		XMLWriter writer = new XMLWriter( new FileOutputStream("src/city.xml"), format );
        writer.write( document );
	}
}
```



```java
		Document document = Dom4JUtils.getDocument();
		//获取根元素
		Element rootElement = document.getRootElement();
		// 	获取根元素下所有子元素
		List<Element> elements = rootElement.elements();
		//根据索引获取第一个city元素
		Element cityelement = elements.get(0);
		System.out.println(cityelement);
		//根据子元素的名称来获取子元素的文本
		String text = cityelement.elementText("Name");
		System.out.println(text);
```

```java
	public static void treeWalk(Element element){
		//输出元素的名称
		System.out.println(element.getName());
		
		List<Element> es = element.elements();
		for (Element e : es) {
			treeWalk(e);
		}
		
	}
```

```java
		Document document = Dom4JUtils.getDocument();
		Element rootElement = document.getRootElement();
		treeWalk(rootElement);
```

```java
		Document document = Dom4JUtils.getDocument();
		Element rootElement = document.getRootElement();
		Element addElement = rootElement.addElement("City");
		addElement.setText("海南");
		Dom4JUtils.Writexml(document);
```

```java
		Document document = Dom4JUtils.getDocument();
		Element rootElement = document.getRootElement();
		List<Element> es = rootElement.elements();
		Element element = es.get(1);
		element.setText("龙门");
		Dom4JUtils.Writexml(document);
```

```java
		Document document = Dom4JUtils.getDocument();
		Element rootElement = document.getRootElement();
		List<Element> es = rootElement.elements();
		Element element = es.get(3);
		Element parent = element.getParent();
		parent.remove(element);
		Dom4JUtils.Writexml(document);
```

```java
		Element element = DocumentHelper.createElement("City");
		element.setText("北京");
		Document document = Dom4JUtils.getDocument();
		Element rootElement = document.getRootElement();
		List<Element> es = rootElement.elements();
		es.add(1, element);
		Dom4JUtils.Writexml(document);
```

```java
		Document document = Dom4JUtils.getDocument();
		Element rootElement = document.getRootElement();
		String value = rootElement.attributeValue("Name");
		System.out.println(value);
```
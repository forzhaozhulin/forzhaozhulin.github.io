---
layout: post
title:  "file"
date:   2017-11-07
categories: Java
tags: file 删除目录
---

* content
{:toc}
## File类

File文件和目录路径名的抽象表示形式.

Java中把文件或者目录（文件夹）都封装成File对象.如果我们要去操作硬盘上的文件，或者文件夹只要找到File这个类即可

#### 构造方法：

```java
	File(File parent, String child) //  根据指定的父路径和文件路径创建File对象
	File(String pathname) // 将指定的路径名转换成一个File对象
	File(String parent, String child)//根据指定的父路径对象和文件路径创建File对象
```

```java
   File f= new File("D:\\a\\a.txt");  //新建一个文件（绝对路径）
   File f = new File ("a.txt"); //在项目的根目录下新建一个文件（相对路径）
```

绝对路径：固定不可改变的路径，以盘符开头

 相对路径：相对某个参照物，不能以盘符开头

​           在eclipse中相对路径相对应当前项目的根目录

### 常用方法：

#### 1.创建和删除

```java
boolean createNewFile():指定路径不存在该文件时时创建文件,返回true,否则返回false
boolean mkdir():当指定的单级文件夹不存在时创建文件夹并返回true，否则返回false  
boolean mkdirs():当指定的多级文件夹某一级文件夹不存在时,创建多级文件夹并返回true,否则返回false
boolean delete():删除文件或者删除单级文件夹（删除一个文件夹，这个文件夹下面不能有其他的文件和文件夹）

```

注意：删除一个文件夹，这个文件夹下面不能有其他的文件和文件夹

```java
 		 File f = new File("d:\\a\\b.txt");//绝对路径
		 File f2 = new File("a.txt");//相对路径
		
		 //boolean createNewFile() : 当指定文件不存在时创建文件并返回true，否则返回false
		 System.out.println(f2.createNewFile());
		
		 
		//boolean mkdir()   : 当指定文件夹不存在时创建文件夹并返回true，否则返回false
		File f3 = new File("b");
		System.out.println(f3.mkdir());
		
		//boolean mkdirs() : 创建指定文件夹，当文件夹所在的目录不存在，则顺道一块创建了
		File f4 = new File("c\\d\\e");
		System.out.println(f4.mkdir());
		System.out.println(f4.mkdirs());
		
		File f5 = new File("c.txt");
		System.out.println(f5.mkdir());
		
		//boolean delete() :当指定的文件或文件夹存在时删除文件或者文件夹 并返回true，否则返回false
		System.out.println(f2.delete());
		System.out.println(f3.delete());

		File f6 = new File("c");
		System.out.println(f6.delete());

```

#### 2.判断

```java
boolean exists():判断指定路径的文件或文件夹是否存在
boolean isAbsolute():判断当前路路径是否是绝对路径
boolean isDirectory():判断当前的目录是否存在
boolean isFile():判断当前路径是否是一个文件
boolean isHidden():判断当前路径是否是隐藏文件
```

#### 3.获取和修改名字

```java
  File getAbsoluteFile():获取文件的绝对路径,返回File对象
  String getAbsolutePath():获取文件的绝对路径,返回路径的字符串
  String getParent():获取当前路径的父级路径,以字符串形式返回该父级路径
  File getParentFile():获取当前路径的父级路径,以字File对象形式返回该父级路径
  String getName():获取文件或文件夹的名称
  String getPath():获取File对象中封装的路径
  long lastModified():以毫秒值返回最后修改时间
  long length():返回文件的字节数
  boolean renameTo(File dest): 将当前File对象所指向的路径 修改为 指定File所指向的路径

```

```java
		f = new File("d:\\a\\b.txt");
		f2 = new File("a.txt");
		
		//File getAbsoluteFile()  ：以File对象的形式返回当前File对象所有指向的绝对路径
		System.out.println(f2.getAbsoluteFile());
		//String getAbsolutePath() : 返回File对象所指向的绝对路径
		System.out.println(f2.getAbsolutePath());
	
```

```java
		File f = new File("d:\\a\\b.txt");
		File f2 = new File("a.txt");

		if(!parent.exists()) {
			parent.mkdirs();
		}
		System.out.println(f3.createNewFile());
		
		//String getParent() 
		System.out.println(f3.getParent());
		//File getParentFile() 
		System.out.println(f3.getParentFile());

```

#### 4.file的其他获取功能

```java
String[] list():以字符串数组的形式返回当前路径下所有的文件和文件夹的名称
File[] listFiles():以File对象的形式返回当前路径下所有的文件和文件夹的名称
static File[] listRoots():获取计算机中所有的盘符
```

```java
		File f = new File("b");
		File f2 = new File("D:\\workspace\\myFile");
		File f3 = new File("c.txt");
		
		//String[] list() : 返回当前路径下所有的文件和文件夹名称
		//注意：只有指向文件夹的File对象才可以调用该方法
		String[] files = f3.list();
		for (int i = 0; i < files.length; i++) {
			System.out.println(files[i]);
		}

```

```java
		File f = new File("b");
		File f2 = new File("D:\\workspace\\myFile");
		File f3 = new File("c.txt");
		
		//File[] listFiles()
		File[] files = f3.listFiles();
		for (File file : files) {
			System.out.println(file.getName());
		}

```

```java
		File[] files = File.listRoots();
		for (File file : files) {
			System.out.println(file);
		}

```

### 删除指定的目录（包含子目录）

```java
import java.io.File;

public class TestDelete {
	public static void main(String[] args) {
		File f = new File("d:\\a");
		method(f);
	}
	
	//删除指定目录下所有文件和目录
	public static void method(File file) {
		if(file.isDirectory()) {
			//干掉自己所有的子文件和子目录
			//获取所有的子文件和子目录
			File[] files = file.listFiles();
			for (File f : files) {
				if(f.isFile()) {
					//直接干掉他
					System.out.println(f.getName());
					f.delete();
				}
				else if(f.isDirectory()) {
					//继续查看是否还有文件和子目录
					method(f);
				}
			}
			
			//干掉自己
			System.out.println(file.getName());
			file.delete();
		}
	}
}

```
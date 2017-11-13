---
layout: post
title:  "Hadoop的“Hello world”---WordCount"
date:   2016-02-14 17:43:54
categories: Hadoop
tags: Hadoop MapReduce Wordcount
author: 薛彬
---

* content
{:toc}

在安装并配置好Hadoop环境之后，需要运行一个实例来验证配置是否正确，Hadoop就提供了一个简单的wordcount程序，其实就是统计单词个数的程序，这个程序可以算是Hadoop中的“Hello World”了。





## MapReduce

### 原理	
	
MapReduce其实就是采用分而治之的思想，将大规模的数据分成各个节点共同完成，然后再整合各个节点的结果，得到最终的结果。这些分节点处理数据都可以做到并行处理，大大缩减了工作的复杂度。

### 过程

MapReduce可以分成两个阶段，其实就是单词拆成map和reduce，这其实是两个函数。map函数会产生一个中间输出，然后reduce函数接受多个map函数产生的一系列中间输出然后再产生一个最终输出。

## WordCount展示

### 前期工作
 
#### 启动hadoop

	cd /usr/hadoop/hadoop-2.6.2/
	sbin/start-dfs.sh
	sbin/start-yarn.sh

#### 创建本地数据文件

	cd ~/
	mkdir ~/file
	cd file
	echo "Hello World" > test1.txt
	echo "Hello Hadoop" > test2.txt
	
这样就创建了两个txt文件，里面分别有一个字符串：Hello World，Hello Hadoop。我们通过wordcount想要得到的结果是这样的：Hello 2，World 1,Hadoop 1。

#### 在HDFS上创建输入文件夹

	hadoop fs -mkdir /input

创建好我们可以通过

	hadoop fs -ls /

来查看结果：

![](http://i.imgur.com/YV2H4DO.png)

#### 将数据文件传到input目录下

	hadoop fs -put ~/file/test*.txt /input

同样，我们可以通过

	hadoop fs -ls /input 

来查看是否上传成功：

![](http://i.imgur.com/awhu8Nl.png)

如果看不到任何结果，说明在hadoop的配置中存在问题，或者是防火墙没有关闭，导致节点连接不通。

### 运行程序

#### 运行wordcount

	hadoop jar /你的hadoop根目录/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.6.2.jar wordcount /input /output

运行这条命令后，Hadoop会启动一个JVM来运行MapReduce程序，而且会在集群上创建一个output文件夹，将结果存在其中。

我们来看看结果：

![](http://i.imgur.com/u6NRid3.png)

注意点：

- 这个目录一定要填对，要不然会报jar不存在。
- 输出文件夹一定要是空文件夹。

#### 查看结果

output文件夹中现在有两个文件，我们需要的结果在`part-r-00000`这个文件夹中。

	hadoop fs -cat /output/part-r-00000

我们就可以看到最终的wordcount结果了：

![](http://i.imgur.com/oqLlB3c.png)

## WordCount源码分析

### Map过程

源码：

```java
import java.io.IOException;
import java.util.StringTokenizer;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Mapper;
public class TokenizerMapper extends Mapper<Object, Text, Text, IntWritable> {
	IntWritable one = new IntWritable(1);
	Text word = new Text();
	public void map(Object key, Text value, Context context) throws IOException,InterruptedException {
		StringTokenizer itr = new StringTokenizer(value.toString());
		while(itr.hasMoreTokens()) {
			word.set(itr.nextToken());
			context.write(word, one);
		}
	}
}
```

继承Mapper类，重写map方法。

我们了解到mapreduce中数据都是通过<key,value>传递的。我们可以通过控制台来看看其中的value值和key值是什么样的。在map方法中加入以下代码：

```java
System.out.println("key= "+key.toString());//查看key值
System.out.println("value= "+value.toString());//查看value值
```
运行程序后控制台输出如下：

![](http://i.imgur.com/1KFbxVu.png)

我们可以看出，map方法中的value值存储的是文本文件中的一行，而key值为该行的首字符相对于文本文件的首地址的偏移量。

程序中的`StringTokenizer`这个类的功能是将每一行拆分成一个一个的单词，并将<word,1>作为map方法的结果输出。

### Reduce过程

源码：

```java
import java.io.IOException;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Reducer;
public class IntSumReducer extends Reducer<Text, IntWritable, Text, IntWritable> {
	IntWritable result = new IntWritable();
	public void reduce(Text	key, Iterable<IntWritable> values, Context context) throws IOException,InterruptedException {
		int sum = 0;
		for(IntWritable val:values) {
			sum += val.get();
		}
		result.set(sum);
		context.write(key,result);
	}
}
```

同样，Reduce过程也需要继承一个Reducer类，并重写reduce方法。

我们可以看到reduce的输入参数是`Text key`和`Iterable<Intrable>`。我们知道reduce方法的输入参数key是一个单词，而values是由各个Mapper上对应单词的计数值所组成的列表，我们可以看到values实现了一个Iterable接口，可以理解成values里面包含了多个IntWritable整数，其实也就是计数值。

然后我们只要遍历values并且求和，就可以得到各单词的总次数了。

### 执行MapReduce

我们已经写好了map函数和reduce函数，现在就是要执行mapreduce了。

源码：

```java
import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
import org.apache.hadoop.util.GenericOptionsParser;
public class WordCount {
	public static void main(String[] args) throws Exception {
		Configuration conf = new Configuration();
		String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();
		if(otherArgs.length != 2) {
			System.err.println("Usage: wordcount <in> <out>");
			System.exit(2);
		}
		Job job = new Job(conf, "wordcount");
		job.setJarByClass(WordCount.class);
		job.setMapperClass(TokenizerMapper.class);
		job.setCombinerClass(IntSumReducer.class);
		job.setReducerClass(IntSumReducer.class);
		job.setOutputKeyClass(Text.class);
		job.setOutputValueClass(IntWritable.class);
		FileInputFormat.addInputPath(job, new Path(otherArgs[0]));
		FileOutputFormat.setOutputPath(job, new Path(otherArgs[1]));
		System.exit(job.waitForCompletion(true)?0:1);
	} 
}
```

代码中的`job.set*()`方法是为对任务的参数进行相关的设置，然后调用`job.waitForCompletion()`方法执行任务。
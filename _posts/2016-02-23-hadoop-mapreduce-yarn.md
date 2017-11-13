---
layout: post
title: "MapReduce1和Yarn的工作机制"
date: 2016-02-23 12:43:00
categories: Hadoop
tags: Hadoop MapReduce
author: 薛彬
---

* content
{:toc}

Hadoop中的MapReduce的工作机制分为两种：

- MapReduce 1 也就是Hadoop 2.0之前的工作机制
- YARN 




## MapReduce 1

### 构成

MapReduce 1最主要的其实就是jobtracker和tasktracker：

- jobtracker，用来协调作业的运行。它也是一个Java程序，主类是JobTracker。
- tasktracker，用来运行作业划分后的任务。它也是一个Java程序，主类是TaskTracker。

除此之外，MapReduce 1的最顶层包括的实体还有两个：客户端、分布式文件系统。

### 工作原理

我们先来看一幅图：

![](http://i.imgur.com/JnRLx7L.png)

这幅图就是MapReduce 1的工作原理。

我们可以看到里面有10个步骤，分别来看看这10个步骤都干了些什么。

1. 客户端启动一个job。
2. 从*jobtracker*请求一个新的作业ID。（`getNewJobId()`方法）
3. 检查作业的输出说明并计算作业的输入分片，然后将运行作业所需要的资源都复制到以作业ID命名的目录下。
4. 提交作业，告知jobtracker作业准备执行。（`submitJob()`方法）
5. 初始化作业。创建一个表示正在运行作业的对象，用来封装任务和记录信息。
6. 获取客户端计算好的输入分片，然后为每个分片创建一个map任务。在此步骤的时候还会创建reduce任务、作业创建任务、作业清理任务。
7. tasktracker运行一个简单的循环来定期发送“心跳”给jobtracker，用来充当两者之间的消息通道。
8. 准备运行任务。
	1. 作业的JAR文件本地化。从共享文件系统把作业的JAR文件复制到tasktracker所在的文件系统。
	2. tasktracker为任务创建一个本地工作目录，并把JAR解压到这。
	3. tasktracker创建一个TaskRunner实例。
9. 启动一个新的JVM来运行每个任务。
10.  运行map/reduce。

这就是经典的MapReduce的工作原理。

## YARN

### 构成

YARN比MapReduce更具一般性，实际上MapReduce只是YARN应用的一种形式。

相比经典的MapReduce来说，YARN的顶层包括更多的实体：

- 客户端。
- YARN资源管理器。负责协调集群上计算资源的分配。
- YARN节点管理器。负责启动和监视集群中机器上的计算容器。
- 应用程序master。负责协调运行MapReduce作业的任务。
- 分布式文件系统。

主要是多了一个容器的概念。每一个任务都有一个对应的容器，而且只能在该容器中运行。

### 工作原理

同样，我们先看一幅图：

![](http://i.imgur.com/PrBOejY.png)

从图中可以看出YARN运行MapReduce的过程有11个步骤，我们分别来看看：

1. 启动一个job。
2. 从*资源管理器*请求一个新的作业ID。
3. 检查作业的输出说明并计算作业的输入分片，然后将作业资源复制到HDFS。
4. 通过调用资源管理器的`submitApplication()`方法提交作业。
5. 将请求传递给调度器。
	1. 调度器分配一个容器。
	2. 资源管理器在节点管理器的管理下在容器中启动master进程。
6. master进程对作业进行初始化。
7. 获取计算出的输入分片，为每个分片创建一个map任务。并创建reduce任务。
8. master为作业向资源管理器请求一个容器来运行任务。
9. 
	1. 调度器为任务分配容器。
	2. master与节点管理器通信来启动容器。
10. 资源本地化。
11. 运行map/reduce任务。

这样一个YARN运行的MapReduce的原理也就完整了。
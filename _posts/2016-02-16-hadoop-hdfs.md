---
layout: post
title:  "Haddop学习笔记---HDFS体系结构"
date:   2016-02-16 18:14:54
categories: Hadoop
tags: HDFS Hadoop
author: 薛彬
---

* content
{:toc}





## HDFS的概念

### 数据块

- HDFS的块默认为64M。
- HDFS上的文件被划分为块大小的多个分块，作为独立的存储单元。
- hadoop fsck / -files -blocks 可以看各个文件由哪些块构成。

### namenode和datanode

- namenode是管理者，datanode是工作者。
- namenode
	- namenode管理文件系统的命名空间。
	- 记录每个文件中各个块所在的数据节点信息。
	- namenode在**内存中**保存文件系统中的每个文件和每个数据块的引用关系。
- datanode
	- 负责所在物理节点的存储管理。
	- 根据需要存储并检索数据块。
	- 一次写入多次读取。

### 读取数据流程

1. 客户端要访问HDFS中的一个文件。
2. 首先从namenode获得组成这个文件的数据块位置列表。
3. 根据列表知道数据块的datanode
4. 访问datanode获取数据。

namenode并不参与数据实际传输。

### 安全模式

```
bin/hadoop dfsadmin safemode enter
```

```
bin/hadoop dfsadmin safemode leave
```


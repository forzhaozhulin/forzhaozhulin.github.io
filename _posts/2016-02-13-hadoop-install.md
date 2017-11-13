---
layout: post
title:  "安装Hadoop需要配置的几个文件"
date:   2016-02-13 12:43:54
categories: Hadoop
tags: Hadoop
author: 薛彬
---

* content
{:toc}


记录安装hadoop时需要配置的几个文件。





## 配置master上的

### hadoop-env.sh

修改JAVA_HOME值
	export JAVA_HOME=/usr/jdk

### yarn-env.sh

修改JAVA_HOME值
	export JAVA_HOME=/usr/jdk

### slaves

加入几个节点的hostname

	slave1
	slave2

### core-site.xml

```xml
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://master:9000</value>
    </property>
    <property>
        <name>hadoop.tmp.dir</name>
        <value>file:/home/hadoop/tmp</value>
    </property>
    <property>
        <name>io.file.buffer.size</name>
        <value>131702</value>
    </property>
</configuration>
```

### hdfs-site.xml

```xml
<configuration>
    <property>
        <name>dfs.namenode.name.dir</name>
        <value>file:/home/hadoop/dfs/name</value>
    </property>
    <property>
        <name>dfs.datanode.data.dir</name>
        <value>file:/home/hadoop/dfs/data</value>
    </property>
    <property>
        <name>dfs.replication</name>
        <value>2</value>
    </property>
    <property>
        <name>dfs.namenode.secondary.http-address</name>
        <value>master:9001</value>
    </property>
    <property>
    <name>dfs.webhdfs.enabled</name>
    <value>true</value>
    </property>
</configuration>
```

### mapred-site.xml
	<configuration>
    <property>
		<name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
    <property>
        <name>yarn.nodemanager.auxservices.mapreduce.shuffle.class</name>
        <value>org.apache.hadoop.mapred.ShuffleHandler</value>
    </property>
    <property>
        <name>yarn.resourcemanager.address</name>
        <value>master:8032</value>
    </property>
    <property>
        <name>yarn.resourcemanager.scheduler.address</name>
        <value>master:8030</value>
    </property>
    <property>
        <name>yarn.resourcemanager.resource-tracker.address</name>
        <value>master:8031</value>
    </property>
    <property>
        <name>yarn.resourcemanager.admin.address</name>
        <value>master:8033</value>
    </property>
    <property>
        <name>yarn.resourcemanager.webapp.address</name>
        <value>master:8088</value>
    </property>
    <property>
        <name>yarn.nodemanager.resource.memory-mb</name>
        <value>768</value>
    </property>
	</configuration>


### yarn-site.xml

	<configuration>
        <property>
               <name>yarn.nodemanager.aux-services</name>
               <value>mapreduce_shuffle</value>
        </property>
        <property>                                                                
			   <name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
               <value>org.apache.hadoop.mapred.ShuffleHandler</value>
        </property>
        <property>
               <name>yarn.resourcemanager.address</name>
               <value>master:8032</value>
       </property>
       <property>
               <name>yarn.resourcemanager.scheduler.address</name>
               <value>master:8030</value>
       </property>
       <property>
            <name>yarn.resourcemanager.resource-tracker.address</name>
             <value>master:8031</value>
      </property>
      <property>
              <name>yarn.resourcemanager.admin.address</name>
               <value>master:8033</value>
       </property>
       <property>
               <name>yarn.resourcemanager.webapp.address</name>
               <value>master:8088</value>
       </property>
	</configuration>

## 将master的配置文件复制到其他节点

	sudo scp -r /usr/hadoop/hadoop-2.6.2 hadoop@slave1:~/

## 启动hadoop
	
在hadoop-2.6.2文件夹下执行
	
	hdfs namenode -format

然后执行

	sbin/start-all.sh

此时执行`jps`在master上可以看到
	
	13077 ResourceManager
	14518 Jps
	12779 SecondaryNameNode
	12444 NameNode

在slave上可以看到

	7126 DataNode
	7336 NodeManager
	7722 Jps


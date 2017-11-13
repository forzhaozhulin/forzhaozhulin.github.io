---
layout: post
title:  "安装Hadoop时：0 datanode(s) running的解决方法"
date:   2016-02-14 16:43:54
categories: Hadoop
tags: Hadoop
author: 薛彬
---

* content
{:toc}





### 发现问题

hadoop安装好之后，通过jps查看进程都正常，但是通过

	hdfs dfsadmin -report

和

	http://master:8088

查看的时候，都没结果，当时也没在意。

在运行wordcount的时候发现文件上传不上去，在日志中发现这样一句话：

	There are 0 datanode(s) running and no node(s) are excluded in this operation.

看来就是安装过程中出现问题。

检查了安装过程中的每一步，都没问题。网上有说是防火墙的问题，也通过关闭iptables来关闭防火墙，还是没有解决。

最后发现，centos7的防火墙不是iptables，默认使用的firewall，所以我始终都没有关闭防火墙。

### 解决问题

首先先关闭防火墙：

	sudo systemctl stop firewalld.service

然后关闭开机启动：

	sudo systemctl disable firewalld.service

如果还想安装iptables并且开放端口就：

	sudo yum install iptables-services

问题就解决了
	
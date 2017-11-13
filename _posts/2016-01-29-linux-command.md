---
layout: post
title:  "Linux常用命令（不全~）"
date:   2016-01-29 19:12:54
categories: Linux
tags: Linux
author: 薛彬
---
* content
{:toc}




## 一些常用的命令

### 网络重启
	sudo /etc/init.d/networking restart

### 切换root用户

	sudo su

### 关闭防火墙
	sudo ufw disable

### 启动防火墙
	sudo ufw enable
	sudo ufw default deny

### 重启Linux	
	sudo reboot
	init6 //root用户才可以使用这一条

### 关闭Linux
	halt
	poweroff
	shutdown -h now //root

### 如何使用vi编辑

按下键盘i就是输入模式

按下esc就是进入命令模式

退出文件
	
	:q
保存后退出文件

	:wq
强制退出文件

	:q!

### 创建文件夹
	mkdir 名称

### 删除空文件夹
	rmdir 名称

### 查看是否可以通信
	ping ip

### 文件复制
	sudo cp

### 文件移动
	sudo mv

### 查看端口是否被占用
	sudo netstat -ap | grep 8080

### ifconfig命令

 ifconfig是linux中用于显示或配置网络设备（网络接口卡）的命令。

禁用网卡：
	
	ifconfig eth0 down
启用网卡：
	
	ifconfig eth0 up

### cd命令

到上一级目录

	cd ../ 
到上二级目录
	
	cd ../..
进入用户个人目录

	cd ~

### 创建用户

创建它

	sudo adduser username
给它root权限

	sudo vi /etc/sudoers
修改文件：

	#User privilege specification
	root ALL=(ALL)ALL
	username ALL=(ALL)ALL
---
layout: post
title:  "一步一步配置ssh免密码登陆"
date:   2016-02-04 18:43:54
categories: Linux
tags: ssh
author: 薛彬
---

记录一下再配置Hadoop中遇到的SSH的原理和配置方法。




### ssh免密码登陆的原理

ssh是每一台Linux的标准配置，它是一种网络协议，用于计算机之间的加密。

首先，我们得在Linux中安装ssh

	sudo apt-get install ssh

ssh的原理其实很简单，就是三个步骤：

1. 用户连接远程计算机，远程计算机收到登陆请求后发送公钥给用户计算机。
2. 用户使用这个公钥将自己输入的登陆密码加密后发送过去。
3. 远程计算机用私钥解密登陆密码，如果正确就同意用户登陆。

其实就是一个公钥和私钥的概念，可以理解为公钥就是设置的用户和其他用户都可以使用的密码，而私钥只要设置的用户自己才知道，都可以用来解密。

上面的步骤中，客户机每次访问远程计算机都需要输入密码，难免会繁琐，ssh提供了一种免密码登陆的方法。

原理其实很简单：

1. 用户在客户机创建一对密钥，将公钥存在需要登陆的服务器（远程计算机）。
2. 用户访问的时候，会向服务器请求自己的密钥进行身份验证。
3. 服务器收到登陆请求之后，就会在服务器上寻找相应的公钥，如果和用户发送的一致就通过。

实现起来其实也很简单：

1. `ssh-keygen -t rsa -P ""`生成公钥文件和私钥文件，分别是id_rsa.pub和id_rsa。这两个文件保存在`~/.ssh`目录下。
2. 将id_rsa.pub的内容追加到authorized_keys的后面。`cat id_rsa >> ~/.ssh/authorized_keys`。
3. `scp id_rsa.pub name:/home/name/.ssh/id_rsa.pub.node1`将公钥发送到用户名为name的.ssh目录下
4. `cat id_rsa.pub.node1 >> authorized_keys`将客户机的公钥追加到服务器的authorized_keys后面。
5. 此时我们还需要解决权限问题。运行命令`sudo chmod 644 ~/.ssh/authorized_keys`和`sudo chmod 700 ~/.ssh`。
6. 此时客户机就可以免密码登陆服务器了。

---
layout: post
title:  "轻松入门Github"
date:   2016-01-24 19:43:54
categories: Github
tags: Github
author: 薛彬
---

* content
{:toc}


第一次听到Github这个词是来自于一个大神[Phodal](https://www.phodal.com/)。那时候因为刚准备学习编程还在纠结该如何学习Java，所以询问了他，他就和我说看懂基础之后多刷刷Github吧，然而在很久以后我才因为想制作一个blog知道了github.io，就开始学着用Github。

现在我也还处于Github入门阶段吧，只会一些常用的指令和操作，怕忘了就记录一下。
参考了《GitHub入门与实践》和《[Github漫游指南](http://github.phodal.com/)》。





## 安装Git

### 进入Git世界

首先当然是要安装Git环境咯，在Git的官网我们可以很容易的找到[下载链接](http://git-scm.com/download/)，对应自己的系统下载即可。

### 初始化

我们需要设置使用Git时的姓名和邮箱地址，这个是在[Github官网](https://www.github.com)注册时使用的。初始化命令如下：

```bash
$git config --global user.name "Your name"
$git config --global user.email "Your email"
```

### 设置SSH Key

首先我们运行如下命令：
	
	$ssh-keygen -t rsa -C "Your email" 

回车之后会弹出几行，是关于ssh保存路径和密码的，默认直接回车就好了。

```bash
Generating public/private rsa key pair.
Enter file in which to save the key
Enter passphrase
Enter same passphrase again
```

之后就会出现has been saved in 等balabala一堆字母，就是成功了。默认保存路径是在/User/你的用户名/.ssh/ 这个文件夹底下。

我们需要用到id_rsa.pub这个文件里面的内容，用EditPlus等编辑软件打开，当然你可以用记事本，Ctrl+A、Ctrl+C全选复制。

然后我们打开[Github](https://www.github.com)，登录刚刚注册的账号，别告诉我你忘了。找到Settings，对，就是你设置头像那，点击右上角小头像就有了。

在左边的一栏中有一项SSH keys，然后点击Add SSH keys，复制进去就好了，名字随便取。

到这我们的Git环境就算是搭建好了。


## 基本操作
 
### 创建仓库

这就不说了。。。在网站上找到New repository就可以了...

### 常见指令

#### 初始化仓库

我们在本地创建一个文件夹后，如果想传到Github上，就需要对它初始化。

```
mkdir demo
cd demo
$git init
```

就好了。

#### 向暂存区中添加文件

	$git add .

#### 保存仓库的历史记录

	$git commit -a -m "Fisrt blood"

#### 推送到远程仓库

这里麻烦一点，需要先添加一个远程仓库

	$git remote add origin https://github.com/yourname/demo.git

然后就可以推送了

	$git push -u origin master

接着输入用户名和密码就好了

#### 获取远程仓库

	$git clone https://github.com/yourname/demo.git

#### 更新仓库

	$git pull


先写到这吧，我也只用到这么多功能了。。。




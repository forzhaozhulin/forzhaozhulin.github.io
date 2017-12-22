---
layout: post
title:  "tomcat"
date:   2017-12-20
categories: web
tags: tomcat eclipse集成tomcat
---

* content
{:toc}
# Tomcat

Tomcat的介绍：Tomcat是Apache 软件基金会（Apache Software Foundation）的Jakarta 项目中的一个核心项目，由Apache、Sun 和其他一些公司及个人共同开发而成。由于有了Sun 的参与和支持，最新的Servlet 和JSP 规范总是能在Tomcat 中得到体现，Tomcat 5支持最新的Servlet 2.4 和JSP 2.0 规范。因为Tomcat 技术先进、性能稳定，而且免费，因而深受Java 爱好者的喜爱并得到了部分软件开发商的认可，成为目前比较流行的Web 应用服务器。

<!-- more -->

 在链接浏览器和数据库的时候需要服务器和http协议作为桥梁在链接浏览器和数据库的时候需要服务器和http协议作为桥梁。

## tomcat的安装

官网：http://tomcat.apache.org/

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171220/J3HlL087K7.png?imageslim)

exe文件是window操作系统下的安装版本

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171220/h784ID7fgg.png?imageslim)

tar.gz 文件 是linux操作系统下的安装版本

exe文件是window操作系统下的安装版本

zip文件是window操作系统下压缩版本

下载后解压

**环境变量：**

Tomcat软件运行它需要依赖Java的运行环境。需要在电脑中配置JAVA_HOME环境变量。 JAVA_HOME环境变量中配置的JDK的安装目录，不能包含bin目录

进入tomcat的安装目录中，在bin目录下有startup.bat文件，双击运行。

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171220/1E8jJ6eAFa.png?imageslim)

打开浏览器在，在浏览器的地址栏中输入：  <http://localhost:8080>

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171220/hLggIKb054.png?imageslim)



## eclipse集成tomcat

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171220/GAlb6cdhiF.png?imageslim)

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171220/g432FLhB39.png?imageslim)

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171220/ILgc80G7Bk.png?imageslim)



![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171220/ALm6e1gmIj.png?imageslim)

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171220/J0elFJfgeL.png?imageslim)

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171220/K6c85LJ7e1.png?imageslim)

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171220/0CeCDJIf5b.png?imageslim)



![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171220/eheLK81CHI.png?imageslim)

如果没出现tomcat服务器，那么：右键 new Server:

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171220/LFf4K3iF4d.png?imageslim)

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171220/8ddaJme7fj.png?imageslim)

配置tomca信息

双击打开，修改配置：

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171220/KKH6hhhkHL.png?imageslim)

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171220/Fe08dBAdIa.png?imageslim)

ctrl + s 保存

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171220/l435m0CGkK.png?imageslim)

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171220/6K148dgAJ5.png?imageslim)

Eclipse创建web项目

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171220/iec68ik144.png?imageslim)

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171220/mge2CIFf8J.png?imageslim)

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171220/8Gm30ffbbh.png?imageslim)

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171220/4h16LdIE60.png?imageslim)

项目的目录介绍：

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171220/3L1d7cH5dh.png?imageslim)

 使用eclipse将项目发布到tomcat都做了那些事呢1.elipse 会把src中java文件编译成class文件, 放到 WebRoot/WEB-INF/classes 目录下.2.eclipse将WebRoot 复制粘贴到tomcat/webapps目录下, 并且将 WebRoot 改名为 项目名
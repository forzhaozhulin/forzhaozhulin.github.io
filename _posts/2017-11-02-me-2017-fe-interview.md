---
layout: post
title:  "我的秋招经历，痛并快乐着"
date:   2017-11-02 18:11:00
categories: 心情
tags: 碎碎念
author: 薛彬
---

* content
{:toc}

随着最后一个面试的结束，手上还剩了一份简历，留作纪念吧。





## 第一章 开始

说实话，我准备秋招的时间不算早，甚至有些迟。

2017年6月份，我意识到互联网巨头的校招快要开始了，然而，那时候没有危机感，觉得自己可以找到一份满意的工作。

也就是这个想法，导致自己的秋招特别坎坷。

**其实，自己真的很菜。**

以后要牢记这句话。

2017年7月4日，阿里巴巴启动了2018届校园招聘，我将准备好的简历发给了朋友让他帮忙内推一下。

看着简历评估中变成了待安排面试。

此时的我才发现自己还没真正系统性地复习过学过的东西。

...

2017年7月13日，阿里的一面电话面，13分钟21秒，全程手抖中打出GG。

这才，真正开始了自己的秋招。

## 第二章 准备

我给自己的第一份工作定位是前端开发工程师。

那就开始看书吧，先列个书单：

- 《JavaScript高级程序设计》
- 《JavaScript语言精粹》
- 《你不知道的JavaScript（上卷）》
- 《ECMAScript 6入门》
- 《深入理解ES6》
- 《深入React技术栈》
- 《学习JavaScript数据结构与算法》
- 《图解HTTP》

其实自己还是没有系统性的看书，而且7 8月份的时候也还是紧迫感不够，导致效率很低。

好记性不如烂笔头。那就记笔记吧。

### 前端方面

之前就看完了阮一峰大神的《ECMAScript 6入门》，在社区中看到《高程》的作者尼古拉斯大神出了本《深入理解ES6》，毫不犹豫上京东上预约了一本，就开始看了，看的过程主要就是记笔记吧：[读《深入理解ES6》笔记](https://github.com/axuebin/understanding-es6-notes)。

因为自己之前的项目多是传统的前后端分离的项目，也就是那种上古时代的前端。

现在的前端工程化、`SPA`、模块化等概念，都只停留在读社区文章的阶段，没有动手实施过。所以就考虑自己写一个项目试试。

那段时间刚好在看 `React`，那就整个 `React` 的项目呗。

为了熟悉开发流程，先是写了几个小 `Demo` （嘘。真的很简单。。）：

- [React实现的简单todolist](https://github.com/axuebin/react-todolist)
- [React实现列车站点查询](https://github.com/axuebin/train-query)
- ...

然后就开始构思重构自己的个人主页了，最后写了一个这个：[使用React重构自己的博客](https://github.com/axuebin/react-blog)，其实也是很简单的一个 `SPA` 应用，算是用来学习 `React` 全家桶的一个 `Demo` 吧。

在撸代码的过程中，就是会经常看看书，知道每一条代码为什么要这样写，理解 `React` 的思想等等，自己动手写过之后再把书看一遍就会能有很好的理解，其中也就写了几篇笔记，都算是加深自己的理解吧。

- [用React实现一个简易的TodoList](https://github.com/axuebin/articles/issues/2)
- [React中state和props分别是什么？](https://github.com/axuebin/articles/issues/4)
- [React的生命周期到底是怎么一回事？](https://github.com/axuebin/articles/issues/5)
- [React V15.6 实现一个简单的个人博客](https://github.com/axuebin/articles/issues/9)

其他的准备就是看书了，然后就是每天刷刷社区，看看大神们的理解和讨论，了解现在主流的前端大环境，这对后来的面试也是很有帮助的。

这里推荐几个觉得比较好的看文章的地方吧：

先自荐一下：[https://github.com/axuebin/articles](https://github.com/axuebin/articles)

- segmentfault：[https://segmentfault.com/](https://segmentfault.com/)
- 掘金社区：[https://juejin.im/](https://juejin.im/)
- 众诚翻译：[http://www.zcfy.cc/article/archive](http://www.zcfy.cc/article/archive)
- 淘宝前端团队：[http://taobaofed.org/](http://taobaofed.org/)
- 京东凹凸实验室：[https://aotu.io/notes/](https://aotu.io/notes/)
- 1024大神的博客：[https://github.com/jawil/blog/issues](https://github.com/jawil/blog/issues)
- camsong：[https://github.com/camsong/blog/issues](https://github.com/camsong/blog/issues)
- 小胡子哥性能专栏：[https://github.com/barretlee/performance-column/issues](https://github.com/barretlee/performance-column/issues)
- JavaScript深入系列以及专题系列：[https://github.com/mqyqingfeng/Blog](https://github.com/mqyqingfeng/Blog)
- 民工精髓大大的博客：[https://github.com/xufei/blog/issues](https://github.com/xufei/blog/issues)
- Kimi Gao：[https://github.com/muwenzi/Program-Blog/issues](https://github.com/muwenzi/Program-Blog/issues)
- Creeper：[https://github.com/creeperyang/blog/issues](https://github.com/creeperyang/blog/issues)
- 前端精读周刊：[https://github.com/dt-fe/weekly](https://github.com/dt-fe/weekly)
- 染陌同学的Vue源码解析：[https://github.com/answershuto/learnVue](https://github.com/answershuto/learnVue)

嘿嘿，现在很多大神都喜欢在 `Github` 的 `issues` 里写 `blog`，要去多多发现哦。

### 数据结构和算法

这方面自己真的不靠谱，只能硬着头皮学一下常用的一些数据结构和算法，主要就是看《学习JavaScript数据结构与算法》和《王道考验：数据结构》。

惭愧的说，后者只刷了几页。。。

### 网络

这方面主要是看了《图解HTTP》，然后就是网上零零散散的文章了，包括知乎上的一些讨论，基本对这方面能有一个了解了。

## 第三章 投递简历

开始了疯狂投简历的阶段。

那时候的想法是，想去有赞。

那时候的有赞，还没开启校招，还没开始宣传。

基本就是在网上找各大互联网公司的内推，还有就是在网上找杭州互联网独角兽公司。。。

内推，网申。。。

## 第四章 笔试面试

人物：axuebin
等级：1级
经验：1%
offer：0

开始了漫长的笔试面试的升级之旅。。。

最初的时候，牛客上满屏的“求内推”。

过了一个月，满屏的“求面经”。

又过了一个月，满屏的“offer比较”。

然而我，打开我的个人资料看了看：

人物：axuebin
等级：5级
经验：15%
offer：0

难受。。

期间总结了一些经验，也确实学到了很多，明白了更多自己的不足，确实是升级了不少。

对于前端来说，在这段时间的面试中知道了面试的重点是什么，比如继承、跨域、性能优化等等。

还有一些加分项：node、移动端开发等等。

这些都是宝贵的经验，自己只能不停地学习学习。。。

期间，阿里巴巴网申面试挂了，大众点评面试挂了，网易网申内推笔试都没通过。。。

还有无数的感谢信。。。

看着自己的等级来到了10级。。。

也收获了两个小offer，也算是有点底了。

时间来到有赞空中宣讲会，HR小姐姐好心地送了一张面试直通卡，我想着我又有机会了。

看书看书再看书，等待着有赞的面试。

...

能力不足。还是挂了。有缘再见。

收到有赞感谢信之后感觉全身轻松，这两个月从来没这么轻松过。。

那天晚上，面了一个公司的电话二面，后来又去现在面了一轮技术面，最后也拿到了这个公司的offer。

## 第五章 尾声

十月份的这段时间，外公去世了，真的很难受，很难受最后的时候自己不在身边。

由于周五和周一都有面试，周六早上一大早就赶回去，周天下午又赶回来，也没能多陪陪我妈妈。

现在也能和外公说一声，我找到满意的工作了。

经历了一个还算是完整的秋招，痛并快乐着。

最后附上我的博客，还有面经（待补充，而且有的二面记不清问什么了就没写了，主要是一面）：

- 文章归档：[https://github.com/axuebin/articles](https://github.com/axuebin/articles)
- 面经：[https://github.com/axuebin/fe-interview-questions](https://github.com/axuebin/fe-interview-questions)


谢谢所有支持我的人。
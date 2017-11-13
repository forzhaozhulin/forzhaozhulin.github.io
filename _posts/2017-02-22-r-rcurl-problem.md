---
layout: post
title:  "RCurl并行发送多个请求导致内存增长的解决方法"
date:   2017-02-22 14:14:54
categories: 其它
tags: R 并行请求 内存管理
author: 薛彬
---

* content
{:toc}


项目中需要向百度地图发送请求获取坐标，用到了R语言下的`RCurl`包。使用该包进行并行发送多个请求，导致消耗的时间随着数据量的增大越来越长，所以有了这篇文章...





## 并行发多个请求

之前的程序是一条一条地发，就想到能不能同时发多个请求，这样就可以将时间缩短。

在`RCurl`的官方文档中发现了解决方法，代码如下：

```R
getURIs = function(uris, ..., multiHandle=getCurlMultiHandle(), .perform = TRUE)
{
    content = list()
    curls = list()
    for(i in uris) {
        curl = getCurlHandle()
        content[[i]] = basicTextGatherer()
        opts = curlOptions(URL = i,forbid.reuse=TRUE,writefunction = content[[i]]$update)
        curlSetOpt(.opts = opts, curl = curl)
        multiHandle = push(multiHandle, curl)
    }  
    if(.perform) {
        complete(multiHandle)
        return(lapply(content, function(x) x$value()))
    } else {
        return(list(multiHandle = multiHandle, content = content))
    }
}
```

有了`getURIs`这个函数，同时发多个请求就很简单了。这个函数的原理其实就是，`getCurlHandle()`会创建一个新的`Handle`，然后存储到通过`getCurlMultiHandle()`创建的`multiHandle`中，然后`complete(multiHandle)`一次发送多条请求。

我们只要这样：

```R
urls.temp<-NULL
for(i in 1:length(data)) {
    location = data[i] 
    url <- paste("http://api.map.baidu.com/geocoder/v2/?ak=",AK,"&output=json&address=",location,"&city=","杭州市", sep = "")
    url_string <- URLencode(url)
    urls.temp<-c(urls.temp,url_string)
    if((i%%500==0)||(is.na(data[i+1]))){
        result<-getURIs(urls.temp)
        urls.temp<-NULL
    }
}
```

当执行上述代码时，先是往`urls.temp`存地址，每500个就将它进行一次`getURLs`，就会同时发500个请求。

## 内存管理

在做完这步之后，发现前10000条数据消耗的时间特别少，随着数据量的增大，时间就慢慢增多了，这就让我觉得可能还有内存方面的原因。

### 重用Handle(无效)

通过help()查看`getURLs`中的`curl = getCurlHandle()`：

> These functions create and duplicate curl handles

就说明每有一个地址，就会创建一个Handel。

然后我就想，理论上这些Handle是可以复用的啊，于是就在第一次初始化了500个Handle之后，接下来就是复用这些Handle。

理想是美好的，现实是残酷的。

不知道是哪里出错了，除了第一次的500个，后面的所有请求都获取不到结果。

### rm()gc()

在导师的引导下，稍微了解了一下R的内存管理机制。

> R清理垃圾的机制和JAVA很像，都是在一定时间内自动发现垃圾再集中清理

我注意到了`mHandle`这个变量，它用来存Handle的，所以我在请求前初始化这个参数，然后在请求处理完成之后进行一次`rm(mHandle);gc()`。

理论上进行了`rm(mHandle)`之后内存就释放了，但就是如之前所说，在进行`rm()`之后内存并没有被立即释放，需要过一会儿才被清理，所以我们需要手动运行垃圾处理函数`gc()`立即释放空间。

然而，在做完如上优化之后，发现问题还是没有解决。

### 慎用rbind

我开始在循环中找那些不断增长的变量，猜测是这些变量导致的时间越来越慢。

Google后发现，`rbind()`应该慎用，于是我就想如何优化我的`rbind`。

之前是遍历获取到的结果，将每一行结果`rbind`，显然这样效率低，于是我将数据存在一个向量`a`中，在最后再利用`data.frame(results=a)`这样生成数据框。

我觉得这样应该就没问题了... 你懂的...并没有太大改善...

在R中，即使是动态向向量中进行`c()`操作，也会导致不断分配内存，所以需要在一开始就初始化一个定长向量，而后只要“填坑”就好了：`a <- seq(from=0.0, to=0.0, length=length(data))`

问题可算是解决了...

意识到无论是什么语言，内存管理都非常重要...


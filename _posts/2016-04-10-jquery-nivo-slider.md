---
layout: post
title:  "一个图片轮播插件---Nivo Slider"
date:   2016-04-10 19:33:54
categories: 前端 
tags: jQuery JavaScript
author: 薛彬
---

* content
{:toc}

安利一个jQuery的图片轮播插件~

在一个项目中需要用到图片轮播，就在谷歌上搜了一下，发现这个插件，配置也简单，使用起来几句代码就搞定了。





![](http://i.imgur.com/fS4aVWd.jpg)

图片是用[http://itbilu.com/javascript/jquery/N1SqAAoz.html](http://itbilu.com/javascript/jquery/N1SqAAoz.html)这个博客中的图~

## 使用方法

### 引用js文件

这个轮播插件直接引入一个js文件就可以搞定了，当然，它是基于jQuery的，还需要引入jQuery的js文件。

js文件可以在[https://github.com/gilbitron/Nivo-Slider](https://github.com/gilbitron/Nivo-Slider)这里下载得到，我们需要用到jquery.nivo.slider.pack.js这个文件。

我们可以在[http://www.jq22.com/jquery-info122](http://www.jq22.com/jquery-info122)下载到jquery-2.1.4.min.js这个文件。

```html
<script type="text/javascript" src="/js/jquery-2.1.4.min.js"></script>
<script type="text/javascript" src="/js/jquery.nivo.slider.pack.js"></script>
```

这样就搞定了。

### 引用css文件

我们需要引入插件的默认css文件，有两个文件：公用样式、主题样式。

```html
<link rel="stylesheet" href="/css/nivo-slider.css" type="text/css" media="screen" />
<link rel="stylesheet" href="/css/themes/default/default.css" type="text/css" media="screen" />
```

可以通过修改css文件中对应的参数来达到自己想要的效果。

### html

最重要的还是如何使用这个插件，很简单，几行html代码就可以搞定。

```html
<div class="theme-default">
    <div id="slider" class="nivoSlider">
		<img src="/images/slider/1.jpg" alt="" title="" /> 
        <img src="/images/slider/2.jpg" alt="" title="" /> 
        <!--……-->
    </div>
</div>
```

如果图片很多，一条一条的写html会感觉很麻烦。我们可以这样做：

```html
<script type="text/javascript">
    $(window).load(function () {
        for (i = 1; i < n; i++) {
            $("#slider").append("<img src='../images/" + i + ".jpg' title='" + i + "' alt=''/>")
        }
    });
</script>
```

这样就可以用一条语句载入多张图片。

### 启动插件

现在图片已经载入完毕，我们启动这个插件。

```html
<script type="text/javascript">
    $('#slider').nivoSlider({
        animSpeed: 100,          //图片过渡时间   
        pauseTime: 2000,         //图片显示时间
        pauseOnHover: false,
        manualAdvance: false,
    });
</script>
```

这里就是为这个插件配置参数的地方了，需要怎样的效果在这里设置，其它的地方都可以不用动。

## 配置文件

```javascript
$('#slider').nivoSlider({  
    effect: 'random',               // 过渡效果  
    slices: 15,                     // 切片效果时的数量  
    boxCols: 8,                     // 格子效果时的列数  
    boxRows: 4,                     // 格式效果时的行数  
    animSpeed: 1000,                // 图片过渡时间  
    pauseTime: 5000,                // 图片显示时间  
    startSlide: 0,                  // 从第几张图片开始（第一张为）  
    directionNav: true,             // 是否显示图片方向切换按钮（上一页/下一页）  
    controlNav: true,               // 是否显示图片导航控制按钮（,2,3... ）  
    controlNavThumbs: false,        // 是否使用图片的缩略图做为导航控制按钮  
    pauseOnHover: true,             // 鼠标县浮时是否停止动画  
    manualAdvance: false,           // 是否手动切换  
    prevText: 'Prev',               // 上一页方向切换按钮的显示文本  
    nextText: 'Next',               // 下一页方向切换按钮的显示文本  
    randomStart: false,             // 开始图片是否随机  
    beforeChange: function(){},     // 图片切换前触发函数  
    afterChange: function(){},      // 图片切换后触发函数  
    slideshowEnd: function(){},     // 在所有图片显示完毕后触发函数  
    lastSlide: function(){},        // 在最后一张图片显示完毕后触发函数  
    afterLoad: function(){}         // 图片加载完毕后触发函数  
}); 
```
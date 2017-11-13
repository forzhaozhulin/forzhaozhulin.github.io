---
layout: post
title:  "通过jQuery实现任意点击弹出层外关闭弹出层"
date:   2016-10-18 15:17:00
categories: 前端
tags: JavaScript jQuery
author: 薛彬
---

* content
{:toc}

在之前做项目的时候经常会在主页面上点击某个按钮，右侧弹出一个div输出对应内容的详细信息。此时，我是希望在鼠标点击弹出层外的时候关闭该弹出层，主要思想就是：

- 找到鼠标点击的那个元素
- 判断这个元素是否在指定区域内，其实就是判断它的父元素是不是弹出层
- 如果不是就对弹出层进行`hide`，如果是就不进行任何操作





### 具体实现

该代码需要使用`jQuery`,代码如下：

```javascript
$(document).mousedown(function(e){
    if($(e.target).parent("#info").length==0){
        $("#info").hide();
    }
})
```

#### $(document).mousedown(function(e){})

- `$(document)`就是获取整个网页文档对象，类似于原生的`window.ducument`
- `mousedown`是鼠标事件，是指**当鼠标指针移动到元素上方并按下鼠标按键时**，类似的事件还有：
    - `mouseup`:当在元素上放松鼠标按钮时
    - `mouseover`：当鼠标指针位于元素上方时

#### $(e.target)

`$(e.target)`表示获取点击事件的元素。

#### parent()

`$(e.target).parent("#info").length`是获取当前点击事件元素带有id为`info`的父元素。
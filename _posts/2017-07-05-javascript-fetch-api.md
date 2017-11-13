---
layout: post
title:  "AJAX的替代方法--Fetch API"
date:   2017-07-05 21:00:54
categories: 前端
tags: JavaScript
author: 薛彬
---

* content
{:toc}


今天学到一个新东西，用来替换AJAX的Fetch API~~




> Fetch API提供了一个fetch()方法，它被定义在BOM的window对象中，你可以用它来发起对远程资源的请求。 该方法返回的是一个Promise对象，让你能够对请求的返回结果进行检索。


fetch()简单用法如下：

```javascript
fetch(url).then(function(response){
	return response.json();
}).then(function(data){
	//TODO
}).catch(function(e){
	alert("出错啦");
})
```

任意一步出错都会报错~

es6写法：

```javascript
fetch(url).then(response => response.json())
	.then(data => console.log(data))
	.catch(function(e){
		alert("出错啦");
	})
```

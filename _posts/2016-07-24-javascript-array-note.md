---
layout: post
title:  "JavaScript数组笔记"
date:   2016-07-24 09:07:00
categories: 前端
tags: JavaScript
author: 薛彬
---

* content
{:toc}

在用Echarts画图的过程中，最先是从数据库获取数据，但是后来由于时间比较急，就改成直接读取csv来获取数据，就涉及到大量的对数组的操作。

本文会提到如何将CSV转成Array，如何将CSV转成JS对象以及一些常见的数组操作。。





## CSVToArray

首先就是如何将CSV转成Array，在网上找到了如下的代码：

```javascript
function CSVToArray(strData, strDelimiter) {
    strDelimiter = (strDelimiter || ",");
    var objPattern = new RegExp(
        (
            "(\\" + strDelimiter + "|\\r?\\n|\\r|^)" +
            "(?:\"([^\"]*(?:\"\"[^\"]*)*)\"|" +
            "([^\\" + strDelimiter + "\\r\\n]*))"
        ),
        "gi"
    );
    var arrData = [[]];
    var arrMatches = null;
    while (arrMatches = objPattern.exec(strData)) {
        var strMatchedDelimiter = arrMatches[ 1 ];
        if (
            strMatchedDelimiter.length &&
            strMatchedDelimiter !== strDelimiter
        ) {
            arrData.push([]);
        }
        var strMatchedValue;
        if (arrMatches[ 2 ]) {
            strMatchedValue = arrMatches[ 2 ].replace(
                new RegExp("\"\"", "g"),
                "\""
            );
        } else {
            strMatchedValue = arrMatches[ 3 ];
        }
        arrData[ arrData.length - 1 ].push(strMatchedValue);
    }
    return(arrData);
}
```

我们现在新建一个这样的csv：

|num|name|sex|age|
|---|---|---|---|
|1|张三|男|19|
|2|李四|女|25|
|3|王五|女|26|
|4|赵六|男|20|

在用ajax直接读取该csv时：

```javascript
$.ajax({
    url: "demo.csv",
    dataType: "text",
    contentType: "application/json; charset=UTF-8",
    success: function (data) {
        console.log(data);
    }
});
```

看到如下结果：

![](http://i.imgur.com/qN4sAQc.png)

这显然不是我们想要的，此时我们加上`var arrData=CSVToArray(data);`，再看看控制台的输出：

![](http://i.imgur.com/qbE27nm.png)

对了，这里还有一个控制台的小技巧，这里可以通过`console.table(arrData)`来输出到控制台，会有惊喜哦~

![](http://i.imgur.com/vIQPDKQ.png)

nice，变成我们熟悉的数组了，可能有的人在想，直接变成json不就简单了吗，别急 ~ 

这样一个n行n列的数组就有了，该如何使用了，愚蠢的我自己写了两个函数。

### 按列取值

通过输入原始数组和列数，就可以返回一个由这一列数组除了第一行名字之外的所有值构成的新数组。

```javascript
function getCol(arr, col) {
    var newCount = new Array();
    for (var i = 1; i < arr.length; i++) {
        newCount.push(arr[i][col]);
    }
    return newCount;
}
```

### 按行取值

```javascript
function getRow(arr, row) {
    var newCount = new Array();
    for (var i = 1; i < arr[row].length; i++) {
        newCount.push(arr[row][i]);
    }
    return newCount;
}
```

----------

在对行列取值的时候，又冒出一个问题，如果遇到`undefined`,`NA`,`0`这样无用的数据的时候怎么办呢？

我记得当初我写这个函数时候，用的pop将数据删除，可是现在无效了。。不知道为什么。。

### 删除数组指定元素

在网上随意Google了一下，发现了如下代码：

```javascript
Array.prototype.indexOf = function (val) {
    for (var i = 0; i < this.length; i++) {
        if (this[i] == val)
            return i;
    }
    return -1;
};
Array.prototype.remove = function (val) {
    var index = this.indexOf(val);
    if (index > -1) {
        this.splice(index, 1);
    }
};
```

这样就可以通过`array.remove(array[i])`或是`array.remove("值")`来删除指定元素。

### 删除数组中的重复项

这也是在处理数据中遇到的。

```javascript
function unique(arr) {
    var tmp = new Array();
    for (var i in arr) {
        if (tmp.indexOf(arr[i]) === -1) {
            tmp.push(arr[i]);
        }
    }
    return tmp;
}
```

### 更高效的unique函数

```javascript
function unique(ary) {
  var i=0,l=ary.length,
      type,p,ret=[],
      guid=(Math.random()*1E18).toString(32)+(+new Date).toString(32),
      objects=[],
      reg={ //Primitive类型值Register
          'string': {},
          'boolean': {},
          'number': {}
      };
  for (;i<l;i++) {
    p = ary[i];
    if (p==null) continue;
    type = typeof p;
    if (reg[type]) {//PrimitiveType
      if (!reg[type].hasOwnProperty(p)) {
        reg[type][p] = 1;
        ret.push(p);
      }
    } else {//RefType
      if (p[guid]) continue;
      p[guid]=1;
      objects.push(p);
      ret.push(p);
    }
  }
  i=objects.length;
  while (i--) {//再将对象上的guid清理掉
    p=objects[i];
    delete p[guid];
  }
  return ret;
}
```

## CSVToObject

有的时候不需要用到数组，只要直接将对象数组丢进去就可以了，比如百度地图热力图。

在Github上找到了这样一个插件：jquery.csv.js。

根据文档，我们可以直接通过`var obj = $.csv.toObjects(data);`就可以将csv获取的数据直接转成JS对象了。。。

```
现在在纠结，在这样的数组中，如何根据某一列将所有数组排序、如何删除指定元素等等问题。
```

应该好好拿出数组基础和Json看看了...

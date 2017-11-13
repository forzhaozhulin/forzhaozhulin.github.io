---
layout: post
title:  "JavaScript动态增删表格的行"
date:   2016-05-12 19:13:54
categories: 前端 
tags: jQuery JavaScript
author: 薛彬
---

* content
{:toc}

在项目中有一个需求：根据一个input的值来生成一个动态表格，可以实现在页面上增删行。

这里介绍的是一个通过动态添加`<tr>`标签来实现增加行的效果。





![](http://i.imgur.com/0mslpR4.png)

效果如图所示~

可以点击下方的添加按钮为表格添加新的一行，也可以点击删除标签删除任意行。

### html

```html
<table id="table">
    <thead>
        <tr>
            <th>序号</th>
            <th>年份</th>
            <th>年度投资比例</th>
        </tr>
    </thead>
    <tbody>
    </tbody>
</table>
<div><button type="button" onclick="addTable1()">添加</button></div>
```

先在html页面上创建一个带有表头的table，也可以为它创建一两行。

### 增加行

```javascript
function addTable() {
    var _len = $("#table tr").length;
    $("#table").append("<tr id=" + _len + " align='center'>"
            + "<td>" + _len + "</td>"
            + "<td><input type='text' name='tzbl" + _len + "' id='table_year" + _len + "' /></td>"
            + "<td><input type='text' class='baifenshu' name='tzbl" + _len + "' id='tzbl" + _len + "' />&nbsp%</td>"
            + "<td style='width:10%'><a href=\'#\' onclick=\'deltrTable(" + _len + ")\'>删除</a></td>"
            + "</tr>");
}
```

1. 通过jQuery的选择器选择`id`为table中的`tr`标签，获取它的长度，这也就是表格的列数。
2. 为表格添加新的`tr`和`td`标签。
3. 为按钮绑定一个`addTable()`。

### 删除行

```javascript
function deltrTable(index){
    var _len = $("#table tr").length;
    $("#table tr[id='" + index + "']").remove(); //删除当前行
    for (var i = index + 1, j = _len; i < j; i++)
    {
        var nextYear = $("#table_year" + i).val();
        var nextTzbl = $("#tzbl" + i).val();
        $("#table tr[id=\'" + i + "\']")
                .replaceWith("<tr id=" + (i - 1) + " align='center'>"
                        + "<td>" + (i - 1) + "</td>"
                        + "<td><input type='text' name='tzbl" + (i - 1) + "' value='" + nextYear + "' id='table1_year" + (i - 1) + "'/></td>"
                        + "<td><input type='text' class='baifenshu' name='tzbl" + (i - 1) + "' value='" + nextTzbl + "' id='tzbl" + (i - 1) + "'/>&nbsp%</td>"
                        + "<td style='width:10%'><a href=\'#\' onclick=\'deltrTable1(" + (i - 1) + ")\'>删除</a></td>"
                        + "</tr>");
    };
}
```

需要注意的地方：

- 字符串连接的时候要格外注意。`""`里面如果再有`""`就用`''`。
---
layout: post
title:  "任意Json转无序列表"
date:   2016-06-20 19:30:05
categories: 前端
tags: JavaScript
author: 薛彬
---

* content
{:toc}

网上很多树状插件都是需要固定格式的Json，但自己在生成Json的时候没考虑这些，所以就只能自己拼接字符串来生成无序列表.




比如：

```json
{
    "顶层菜单1":[
        {
            "domain":"顶层菜单1",
            "runType":"background",
            "moduleName":"子菜单",
            "memo":"描述",
            "srcFile":"",
            "depends":[]
        }
    ],
    "顶层菜单2":[
        {
            "domain":"顶层菜单2",
            "runType":"background",
            "moduleName":"子菜单1",
            "memo":"描述",
            "srcFile":"",
            "depends":[]
        },
        {
            "domain":"顶层菜单2",
            "runType":"background",
            "moduleName":"子菜单2",
            "memo":"描述",
            "srcFile":"",
            "depends":[]
        }
    ],
    "顶层菜单3":[]
}
```

直接贴代码吧...

```javascript
dataObject = JSON.parse(data);
var html = '<ul>';
var domainName;
var moduleName;
for ( var n in dataObject) {
    html += '<li>' + n + '<ul class="listItem">';
    for (var i = 0; i < dataObject[n].length; i++) {
        domainName=dataObject[n][i].domain;
        moduleName=dataObject[n][i].moduleName;
        html += '<a href="#">' + '<li>'+ moduleName + '</li>'+ '</a>';
    }
    html += '</ul></li>';
}
html += '</ul>';
$('#sidebar').append(html);
```


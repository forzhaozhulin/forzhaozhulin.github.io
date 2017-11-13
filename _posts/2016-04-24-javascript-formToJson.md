---
layout: post
title:  "jQuery将表单序列化为Json对象（转载）"
date:   2016-04-24 20:10:34
categories: 转载 
tags: jQuery JavaScript
author: 薛彬
---

* content
{:toc}

在一个项目中需要将整个页面的表单保存为一个json字符串，苦恼了好久，然后就找到了这个方法，觉得还是不错的~

安利一个jQuery将表单序列化为Json对象的方法~

[原文地址：http://www.tashan10.com/jquery-jiang-biao-dan-xu-lie-hua-wei-jsondui-xiang/](http://www.tashan10.com/jquery-jiang-biao-dan-xu-lie-hua-wei-jsondui-xiang/)





大家知道`Jquery`中有`serialize`方法，可以将表单序列化为一个`“&”`连接的字符串，但却没有提供序列化为`Json`的方法。不过，我们可以写一个插件实现。

我在网上看到有人用替换的方法，先用`serialize`序列化后，将`&`替换成`：` 、`‘`：

```javascript
/** 
* 重置form表单 
* @param formId  form的id  
*/  
function resetQuery(formId){  
    var fid = "#" + formId;  
    var str = $(fid).serialize();  
    //str= cardSelectDate=3&startdate=2012-02-01&enddate=2012-02-04  
    var ob= strToObj(str);  
    alert(ob.startdate);//2012-02-01  
}  
function strToObj(str){  
    str = str.replace(/&/g,"','");  
    str = str.replace(/=/g,"':'");  
    str = "({'"+str +"'})";  
    obj = eval(str);   
    return obj;  
} 
```

这个方案有个重大bug：如果输入框中本身有`=`怎么办呢？ 所以，我的方法是，先用serializeArray序列化为数组，再封装为Json对象。

**表单：**

```html
<form id="myForm" action="#">
    <input name="name"/>
    <input name="age"/>
    <input type="submit"/>
</form>
```

**jQuery插件代码：**

```javascript
(function($){
    $.fn.serializeJson=function(){
        var serializeObj={};
        $(this.serializeArray()).each(function(){
            serializeObj[this.name]=this.value;
        });
        return serializeObj;
    };
})(jQuery);
```

**测试一下：**

```javascript
$("#myForm").bind("submit",function(e){
    e.preventDefault();
    console.log($(this).serializeJson());
});
```

**测试结果：**

```javascript
输入a，b提交，得到序列化结果
{age: "b",name: "a"}
```

上面的插件，不能适用于有多个值的输入控件，例如复选框、多选的select。下面，我将插件做进一步的修改，让其支持多选。代码如下：

```javascript
(function($){
    $.fn.serializeJson=function(){
        var serializeObj={};
        var array=this.serializeArray();
        var str=this.serialize();
        $(array).each(function(){
            if(serializeObj[this.name]){
                if($.isArray(serializeObj[this.name])){
                    serializeObj[this.name].push(this.value);
                }else{
                    serializeObj[this.name]=[serializeObj[this.name],this.value];
                }
            }else{
                serializeObj[this.name]=this.value; 
            }
        });
        return serializeObj;
    };
})(jQuery);
```

这里，我将多选的值封装为一个数值来进行处理。如果大家使用的时候需要将多选的值封装为“,"连接的字符串或者其他形式，请自行修改相应代码。

**表单：**

```html
<form id="myForm" action="#">
    <input name="name"/>
    <input name="age"/>
    <select multiple="multiple" name="interest" size="2">
        <option value ="interest1">interest1</option>
        <option value ="interest2">interest2</option>
        <option value="interest3">interest3</option>
        <option value="interest4">interest4</option>
    </select>
    <input type="checkbox" name="vehicle" value="Bike" /> I have a bike
    <input type="checkbox" name="vehicle" value="Car" /> I have a car
    <input type="submit"/>
</form>
```

**测试结果：**

```javascript
{age: "aa",interest: ["interest2", "interest4"],name: "dd",vehicle:["Bike","Car"]}
```
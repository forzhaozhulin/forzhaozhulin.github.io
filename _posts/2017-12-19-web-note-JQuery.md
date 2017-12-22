---
layout: post
title:  "jQuery"
date:   2017-12-19
categories: web
tags: jQuery
---

* content
{:toc}
## jQuery

> jQuery是一个快速、简洁的JavaScript框架，是继Prototype之后又一个优秀的JavaScript代码库（*或JavaScript框架*）。jQuery设计的宗旨是“write Less，Do More”，即倡导写更少的代码，做更多的事情。它封装JavaScript常用的功能代码，提供一种简便的JavaScript设计模式，优化HTML文档操作、事件处理、动画设计和Ajax交互。

<!-- more -->

注意： jQuery2.0及后续版本不再支持IE6/7/8浏览器

### 引入jQuery库

学习JavaScript时，我们就学习过自定义JS库的导入，学习jQuery只需要将对应js库下载，并导入到我们项目下，在html页面使用<script>导入即可。

```html
<script src="js/jquery-1.11.0.js" type="text/javascript" ></script>
```

### 对象获取

基本语法：jQuery(选择器)  或  $(选择器)

即在 jQuery中 "jQuery == $"，区分大小写

```js
//获得jQuery对象
var demo = $("#demoId");
```

jQuery和原生js获得的对象是不一样的

### Query对象和DOM转换

jQuery对象和DOM对象可以相互转换，但两个对象的函数不能彼此混搭使用。及jQuery对象只能使用自己的函数

DOM对象转换成jQuery对象，语法： $(dom对象)

jQuery对象转换成DOM对象，语法： username[0]

###jQuery页面加载

jQuery提供ready()函数，用于页面成功加载后执行。与window.onload函数类似。

| JavaScript页面加载完成 ： | window.onload = function() { …  };      |
| ------------------ | --------------------------------------- |
| jQuery页面加载完成方式1：   | $(document).ready( function() { …  } ); |
| jQuery页面加载完成方式2：   | $( function() { … } );                  |

### 基本选择器：

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171219/A22DLgL7dh.png?imageslim)

### 效果：

通过改变元素 高度和宽度 显示或隐藏。

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171219/L7iK0HcH4i.png?imageslim)

show(speed ,fn) 显示

​	参数1  speed，显示速度，单位：毫秒。固定字符串：slow, normal, or fast

​	参数2  fn，   显示成功之后回调函数。

hide()   隐藏

toggle() 切换

#### 滑动

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171219/iAAjiAKB11.png?imageslim)

slideDown()  显示，高度变大。

slideUp()    隐藏，高度变小。

slideToggle() 切换

#### 淡入淡出

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171219/D4i7a09FJC.png?imageslim)

fadeIn()       显示（淡入）

fadeOut()      隐藏（淡出）

fadeToggle()   切换

fadeTo(speed,opacity,[fn]) 指定透明度

参数2 opacity ：一个0至1之间表示透明度的数字

```js
<script type="text/javascript">
  setTimeout(function(){
  //$('img').show(1000); 
  //$('img').slideDown(1000);	
  $('img').fadeIn(2000);
  setTimeout(function(){
    //$('img').hide(1000);
    //$('img').slideUp(1000);
    $('img').fadeOut(2000)
  },3000);
},3000);
</script>
```



### 层级选择器：

| `层级选择器`               | `例子`    | `说明`                      |
| --------------------- | ------- | ------------------------- |
| `ancestor descendant` | `A   B` | `获得A元素内部的所有的B元素。（祖孙）--后代` |
| `parent > child`      | `A > B` | `获得A元素下面的所有B子元素。（父子）`     |
| `prev + nex`          | `A + B` | `获得A元素同级下一个B元素（兄弟）`       |
| `prev ~ sibling`      | `A ~ B` | 获得A元素之后的所有B元素（兄弟）         |

```html
<!-- 获取id为ul1下面所有的li
获取id为ul1的子级
获取id为java的li同级下面的一个元素
获取id为java的li同级下面的所有元素-->
<script src="../js/jquery-1.8.3.min.js"></script>
<script type="text/javascript">
  console.log($('#ul1 li').length);
  console.log($('#ul1>li').size());
  console.log($('#java+li')[0].innerHTML)
  console.log($('#java ~li').length)
</script>
```

### 基本过滤选择器

| :first     | 获取选择的元素中的第一个元素  |
| ---------- | --------------- |
| ：last      | 获取选择的元素中的最后一个元素 |
| :even      | 偶数，从 0 开始计数     |
| :odd       | 奇数，从 0 开始计数     |
| :eq(index) | 指定第几个 =         |
| :gt(index) | 大于index个        |
| :lt(index) | 小于index个        |

```html
<!--
		获取ul下面的第一个子元素
		获取ul下面的最后一个子元素
		获取ul下面索引值为偶数的子元素
		获取ul下面索引值为奇数的子元素
		获取ul下面第三个的子元素
		获取ul下面索引值大于1的子元素
		获取ul下面索引值小于1的子元素
		-->
		<script src="../js/jquery-1.11.0.min.js"></script>
		<script type="text/javascript">
			// 获取ul下面的第一个子元素
			console.log($('ul li:first')[0].innerHTML);
			//获取ul下面的最后一个子元素
			console.log($('ul li:last')[0].innerHTML);
			//获取ul下面索引值为偶数的子元素
			console.log($('ul li:even').size()); 
			// 获取ul下面索引值为奇数的子元素
			console.log($('ul li:odd').size()); 
			// 获取ul下面第三个的子元素
			console.log($('ul li:eq(2)')[0].innerHTML);
			// 获取ul下面索引值大于1的子元素
			console.log($('ul li:gt(1)').size()); 
			//获取ul下面索引值小于1的子元素
			console.log($('ul li:lt(1)').get(0).innerHTML); 
		</script>
```

### 属性选择器

| 属性选择器名称                                  | 翻译       | 说明                                       |
| ---------------------------------------- | -------- | ---------------------------------------- |
| [attribute]                              | [属性名]    | 获得有属性名的元素。                               |
| [attribute=value\]                       | [属性名=值]  | 获得属性名 等于 值 元素。                           |
| [attribute!=value\]                      | [属性名!=值] | 获得属性名 不等于 值 元素。                          |
| [attribute^=value\]                      | [属性名^=值] | 获得属性名 以 值  开头 元素。                        |
| [attribute$=value\]                      | [属性名$=值] | 获得属性名 以 值  结尾 元素。                        |
| [[attribute*=value\]](mk:@MSITStore:D:\lesson\JavaWeb\day05_jQuery（上）\课前资料\jquery1.8.3_api.chm::/attributeContains.html) | [属性名*=值] | 获得属性名 含有 值 元素。                           |
| [attrSel1\][attrSel2][attrSelN]          |          | 复合属性选择器，多个属性同时过滤。首先经[attrSel1]选择返回集合A，集合A再经过[attrSel2]选择返回集合B，集合B再经过[attrSelN]选择返回结果集合。 |

```html
 <script src="../js/jquery-1.11.0.min.js"></script>
		<script type="text/javascript">
			// 获取所有有type属性的元素
			console.log($('input[type]').size()); // 8
			// 获取所有type属性值为’text’的元素
			console.log($('input[type=text]').size()); // 4
			// 获取所有有type属性值不等于’text’的元素
			console.log($('input[type!=text]').size()); // 4
			// 获取所有value属性值以’a’开头的元素
			console.log($('input[value^=a]').size()); // 3
			//  获取所有value属性值以’b’结尾的元素
			console.log($('input[value$=b]').size()); // 2
			//  获取所有value属性值含有’c’的元素
			console.log($('input[value*=c]').size()); // 3
			// 获取所有value属性值以’a’开头并且含有’c’的元素
			console.log($('input[value^=a][value*=c][type=text]').val()); // 
		</script>
```

### 表单对象属性选择器：

| 表单属性选择器的名称 | 描述                  |
| ---------- | ------------------- |
| :enabled   | 获取所有可用元素            |
| :disabled  | 选取所有不可用元素           |
| :checked   | 选取所有被选中的元素，如单选框、复选框 |
| :selected  | 选取所有被选中项元素，如下拉列表框   |

```html
<script type="text/javascript">
			//获取 单选、复选框选中的项
			console.log($('input[type=radio]:checked').val()); // male
			console.log($('input[type=checkbox]:checked').size()); // 2
			// 获取下拉列表下面选中的option
			console.log($('select[name=city] option:selected').val()); // sh
</script>
```

### class的操作

| 方法              | 描述                         |
| --------------- | -------------------------- |
| addClass(类名)    | 给指定标签的class属性追加样式          |
| removeClass(类名) | 将标签指定的class属性移除            |
| toggleClass(类名) | 切换class属性样式。及没有时添加，有的时候删除。 |

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			.myP {
				width: 100px;
				height: 100px;
				background-color: blue;
			}
			.myDiv {
				width: 100px;
				height: 100px;
				background-color: yellow;
			}
		</style>
	</head>
	<body>
		<div>我是div</div>
		<p class="myP">我是p</p>
		
		<script src="../js/jquery-1.8.3.min.js"></script>
		<script type="text/javascript">
			//页面加载的时候给div添加一个样式类(myDiv)
			$('div').addClass('myDiv');
			// 页面加载的时候把p的样式类(myP)
			$('p').removeClass('myP');
			// toggleClass(样式类的名称);  切换样式类：如果有相应的样式 要删除， 如果没有则添加
			$('p').toggleClass()('myP');
			
		</script>		
	</body>
</html>
```

###  属性操作：prop() 和attr()

| 方法名                                      | 描述                                       |
| ---------------------------------------- | ---------------------------------------- |
| attr(属性名)attr(属性名,属性值)attr( {属性名1：属性值1，属性名2：属性值2…} ) | 可以用来获取某一个属性对应的值可以用来设置某一个属性对应的值设置多个属性，以JSON结构传递 |
| removeAttr(属性名)                          | 删除某一个属性                                  |
| prop(属性名)prop(属性名：属性值)prop( {属性名1：属性值1，属性名2：属性值2…} ) | 可以用来获取某一个属性对应的值可以用来设置某一个属性对应的值设置多个属性，以JSON结构传递 |
| removeProp(属性名)                          | 删除某一个属性                                  |

1. 对于HTML元素本身就带有的固有属性，在处理时，使用prop方法。
2.  对于HTML元素我们自己自定义的DOM属性，在处理时，使用attr方法。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<input type="text" name="username" aaa="aaa" />
		<!--
	       获取自定义属性aaa的值
		   设置自定义属性bbb的值为’bbb’ 
		   设置多个自定义属性ccc的值为’ccc’,ddd的值为’ddd’
		   需求四：删除自定义属性 aaa
		   获取固有属性name属性的值
		   设置固有属性id的值为’username’ 
		   设置多个固有属性id的值为’username’,value的值为’Jack’
		   删除固有属性 id
		-->
		<script src="../js/jquery-1.11.0.min.js"></script>
		<script type="text/javascript">
			// 获取自定义属性aaa的值
			console.log($('input').attr('aaa'));
			
			// 设置自定义属性bbb的值为’bbb’
			$('input').attr('bbb','bbb');
			
			// 设置多个自定义属性ccc的值为’ccc’,ddd的值为’ddd’
			// attr({k:v,k:v});
			$('input').attr({
				"ccc" : "ccc",
				"ddd" : "ddd"
			});
			
			// 删除自定义属性 aaa
			$('input').removeAttr('aaa');
			
			// 获取固有属性name属性的值
			console.log($('input').prop('name')); 
			// 设置固有属性id的值为’username’ 
			$('input').prop('id','username');
			
			// 设置多个固有属性id的值为’username’,value的值为’Jack’
			$('input').prop({
				"id" : "username",
				"value" : "Jack"
			});
			
			//  删除固有属性 id
            // $('input').removeProp('id');
			$('input').removeAttr('id');
		</script>
	</body>
</html>
```

删除用removeAttr(),用removeProp()只会将id的变值为undefined

### 属性的操作：val、text、html

val()         获得表单元素value属性的值。

val(…)      给表单元素的value属性设置值。

html()      获得非表单元素的html代码，如果有标签，一并获得。

html(…)   设置非表单元素的html代码，如果有标签，将进行解析。

 text()       获得非表单元素的文本，如果有标签，忽略。

 text(…)    设置文本，如果含有标签，不进行解析。原样输出。

总结：

1、val()相当于我们之前的value 。

2、html()相当于我们之前的innerHTML 。

```html
			<script type="text/javascript">
				//获取文本框的value值
				console.log($('input').val());
				//设置文本框的value值为‘world’
				$('#txt').val('world');
				//获取id为div1的div的标签内容
				console.log($('#div1').html());
				//设置id为div1的div的标签内容为：‘<b>我是加粗的</b>’
				$('#div1').html('<b>我是加粗的</b>');
				//使用text() 获取id为div2的div的标签内容
				console.log($('#div2').text());
				//使用text() 设置id为div2的div的标签内容：‘<b>我是加粗的</b>’
				$('#div2').text('<b>我是加粗的</b>');
			</script>
```

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171219/j1fJmaklCe.png?imageslim)

### 遍历函数：each

1.原始方式遍历：

跟普通JS循环一样，使用普通的for循环，因为每个JQ对象都有一个length属性。

语法结构：

```js
for (var i = 0; i < $ele.length; i++) {
}
```

2、 JQ对象的方法 

此方式是jquery特有的遍历方式，使用JQ对象调用each方法即可遍历。

语法结构：

```js
$ele.each(function(index,element) {
        this
});
each方法接收一个回调函数
回调函数参数：
	index  :  遍历的索引（下标）
    element:  遍历的当前的对象
```

经验：我们一般不会去写回调函数的参数，可以直接在回调函数里面使用this , 而this和element是等价的 ：即this == element。

3、JQ提供的全局方法

​    此方式是jquery特有的遍历方式，与上面jquery的对象方法相似，

​    但此处的each方法不是某个具体jquery对象的，而是jquery的全局对象的each方法。

​    语法结构：

```js
$.each($ele,function(index,element) {
        this
});
该方法接收两个参数：
第一个参数：要遍历的JQ对象
第二个参数：回调函数，语法同上
```

`注意`：别管是上述的哪种遍历方式，遍历的集合中的每一个元素对象都是原生JS对象，而原生JS对象不能使用JQ的所有方法，要想使用JQ的方法要使用$()包一层。

### 文档的处理：内部插入

| `append(content|fn)`  | A.append(B) : <A>....<B></B></A>        | 将B插入到A内部后      |
| --------------------- | --------------------------------------- | -------------- |
| `appendTo(content)`   | **A.appendTo(B)** : <B>....<A></A></B   | **将A插入到B内部后**  |
| `prepend(content|fn)` | **A.prepend(B) :** <A><B></B>....</A>   | **将B插入到A内部前面** |
| `prependTo(content)`  | **A.prependTo(B) :** <B><A></A>....</B> | **将A插入到B内部前面** |

```html
<script type="text/javascript">
			// JQ ： $('完成的标签名'); $('<li>..</li>')
			// 创建一个li标签并且使用append追加到ul的最后面
			// A.append(B); 将 B 插入到A 内部的后面
			$('ul').append($('<li>222</li>'));
			
			// 创建一个li标签并且使用prepend添加到ul的最前面
			// A.prepend(B); 将B插入到A内部的前面
			$('ul').prepend($('<li>000</li>'));
			
			// 创建一个li 并且使用appendTo添加到ul的最后面
			// A.appendTo(B) 将A插入到B内部的后面
			$('<li>333</li>').appendTo($('ul'));
			
			// 创建一个li 并且使用prependTo添加到ul的最前面
			// A.prependTo(B) 将A插入到B内部的前面
			$('<li>-1-1-1</li>').prependTo($('ul'));
		</script>
```

### 文档的处理：外部插入

| after(content\|fn)        | A.after(B)  : <A></A><B></B>           | 将B插入到A外部的后面     |
| ------------------------- | -------------------------------------- | --------------- |
| **before(content\|fn)**   | **A.before(B) :** <B></B><A></A>       | **将B插入到A外部的前面** |
| **insertAfter(content)**  | **A.insertAfter(B) :** <B></B><A></A>  | **将A插入到B外部的后面** |
| **insertBefore(content)** | **A.insertBefore(B) :** <A></A><B></B> | **将A插入到B外部的前面** |

```html
<script type="text/javascript">
			//创建一个li使用after插入到bbb的后面
			$('#bbb').after('<li>ccc</li>');
			//创建一个li使用before插入到bbb的前面
			$('#bbb').before('<li>aaa</li>');
			//创建一个li使用insertAfter插入到bbb的后面
			$('<li>111</li>').insertAfter('#bbb');
			//创建一个li使用insertBefore插入到bbb的前面
			$('<li>222</li>').insertBefore('#bbb')
		</script>
```

### 样式的操作：css 

​        行间的样式级别最大，css操作的就是行间样式

**语法结构：**

$ele.css(样式名);          获取指定标签的指定样式 

$ele.css(样式名，样式值);   给指定标签设置一个样式 

$ele.css({样式名1：值1，样式名2：值2}); 给指定标签设置多个样式 

```html
		<script type="text/javascript">
			//获取div 的宽度
			console.log($('div').css('width'))
			//给div设置一个红色的背景色
			$('div').css('backgroundColor','red')
			//给div设置多个样式：2px 实心的 灰色边框 和 字体颜色为白色
			$('div').css({
				"border":"2px solid gray",
				"color" : "white"
			});
 		</script>
```

###  元素的删除、清空： remove、empty

语法结构：

$ele.remove()； 删除指定的元素

$ele.empty();   清空指定元素的内容，标签保留

DOM的添加操作相当于一个剪切的功能

### this: 

当前的方法属于谁，看调用，谁调用就指向谁

1. 事件函数里面的this

   oBtn.onclick = function() {

   ​	this； 当前的事件源对象：当前事件发生的源头

   };

2. 全局函数里面的this: window 

   全局的东西都属于window 而window. 可以省略

   ​	funciton show() {

   ​		alert(this);

   ​	}

   ​	window.show();

​	Jq的this：

​		$(this)

### 事件：

```js
$ele.事件名(function() {
});
注意：事件名需要把原生JS的事件的“on”去掉
例如：oDiv.onclick = function() {…};  ->  $div.click(function(){…});
```

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171219/ldHBb16mm7.png?imageslim)

* hover( over , out )  简化方法 

  A.hover( fn1 ,  fn2)  等效与  A.mouseover( fn1 ).mouseout( fn2 ) 

* toggle( fn1 , fn2 , .... )  

  click事件增强版，轮回执行里面的函数。 

  注意：1.8.3之前版本可用，其他高版本不可用 

#### 事件的绑定与解绑定

原生JS里面的方法绑定与解绑定：

```html
<body>
	<button>点我试试</button>
	<script type="text/javascript">
		var oBtn = document.body.children[0];
		// 给按钮绑定一个点击事件
		oBtn.onclick = function() {
			console.log('hello world');
		};
		
		// 将按钮的点击事件解除
		oBtn.onclick = null;
	</script>
</body>
```

jQuery里面事件的绑定与解绑定：

```js
事件的绑定:
    $("button").bind({
    click:function(){ … },
    mouseover:function(){ … }, 
    mouseout:function(){ … } 
    });
解绑:
解绑所有事件：
    $("btn").unbind();
解绑指定事件：
    $("button").unbind(“click”);
```

```html
<body>
		<button>点我</button>	
		<script src="../js/jquery-1.8.3.min.js"></script>
		<script type="text/javascript">
			$('button').bind({
				click: function(){
					alert(1);
				},
				mouseover: function(){
					$('button').css('background-color','blue');
				},
				mouseout: function(){
					$('button').css('background-color','red');
				},
			});
			// 使用unbind方法解除按钮的点击事件
			$('button').unbind('click');
			//解除所有事件
			$('button').unbind()
		</script>
	</body>
```

### validation校验插件

语法格式：

```js
$("form表单的选择器").validate({
rules : {
       表单项name值：{
校验规则
}，
       表单项name值：{
校验规则
}，
... ...
 	 },
 	 messages : {
       表单项name值：{
错误提示信息
}，
       表单项name值：{
错误提示信息
}，
... ...
 	 }
});
```

说明：

1. jQuery Validate需要手动的声明对那个表单进行校验，

​    即需要手动调用validate()方法。

2. rules： 配置验证规则（JSON结构）

​         里面是具体的验证规则（JSON结构），

​         key是被验证表单元素的name属性的值， 

​          value是验证规则 (也是json格式)

3. messages： 配置通过验证规则不通过要显示的提示信息（JSON结构）

​         里面是具体的错误信息（JSON结构），

​         key是被验证表单元素的name属性的值，

​         value是提示信息 (也是json格式)

#### 默认的 校验规则:

![mark](http://ovct5gg6c.bkt.clouddn.com/blog/171219/777j6FBFkB.jpg?imageslim)
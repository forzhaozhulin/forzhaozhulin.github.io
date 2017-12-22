---
layout: post
title:  "HTML"
date:   2017-12-18
categories: web
tags: html 标签
---

* content
{:toc}
## HTML

HTML：超文本标记语言，HyperText Markup Language。就是通过标签对儿，给纯文本增加语义。也就是说，用文本给文本增加语义，所以这个叫做“`超文本`”。而有一对儿对儿标签，也成为“标记”，所以就是“`超文本标记语言`”。

<!-- more -->

### html特点：

1.html不需要编译，直接使用浏览器解析

2.扩展名“.html”

3.结构由标签组成

4.标签预先定义好，

### 标签介绍：

#### 标题标签：

```html
<hn> n 1-6之间的数 块级元素
<h1></h1>最大
<h6></h6>最小
```

#### 水平线标签：

```htm
<hr/>
属性：
  size : 大小
  noshade ： 没有阴影 纯色
```

#### 字体标签；

```html
<font></font>
  size ： 字体大小 1-7  7最大
  color ： 字体的颜色 
	color ： 字体的颜色 
1. #xxxxxx 16进制的颜色
    #000000 黑色 简化：#000
    #FFFFFF 白色 简化：#fff
    #FF0000 红色 简化： #f00
2.colorname 颜色的英文单词
red yellow blue green

face ： 字体
```

#### 格式化标签：

```html
<b></b> 加粗的 
<i></i> 斜体的
```

#### 段落标签：

```html
<p></p>
<br/> 换行
```

#### `图片标签`：

```html
<img src=“图片的路径地址”/>
注意：路径不能是中文
width :设置图片的宽度
height：设置图片的高度
alt ：图片加载不出来的时候，显示alt属性中的值
title ：鼠标放在图片上显示文字
```

#### 特殊字符：

```html
空格  &nbsp；
>      &gt;
<      &lt;
 版权符号  &copy;
```

在html中以上字符都有特殊含义，无法直接显示，所以需要用其他组合字符代替。

#### `列表标签`：

```html
无序列表<ul>
<ul type="disc">
        <li></li>
        <li></li>
</ul>
disc : 默认黑心圆
circle： 空心的
square ：黑 方形

有序列表：<ol>
<ol type ="a">
        <li></li>
        <li></li>
</ol>
默认1234
"a" abcd
罗马数字 i/I

在编辑器中的快捷键：ul>li*5 然后 tab
```

#### `超链接标签`：

```html
<a href="跳转路径"></a>
target ： 跳转方式
默认 _self本页面打开
       _blank 新页面打开
<a href="http://www.baidu.com></a>
<a href="http://www.baidu.com target="_blank" ></a>

空连接
<a herf="#"></a>
<a herf="javascript:;"></a>   推荐
<a herf="javascript:void(0);"></a>

锚点连接：
<a herf="#pig"></a>
<p></p>
<p></p>
<p></p>
<a name="pig"></a>
```

#### `表格标签`：

```html
<table> 表格
<tr> 行
<td> 列
一个table可能包括多个<tr> <td>

table：
boder：设置表格的边框
cellspaceing ： 设置单元格和单元格之间的距离
cellpadding ： 单元格和内容之间的距离
align ： 表格居中

tr：
    align ：行内所有单元格中内容的对齐方式
bgcolor： 行的背景颜色

td：
    align ：列内所有单元格中内容的对齐方式
    rowspan ：实现单元格的跨行
    colspan    ： 实现单元格的跨列
```

#### `输入域标签`：

```html
<input>：
  type：
  <input type="text"  /> 文本框
  <input type="password" /> 密码框
  <input type="radio" />   单选按钮 name 属性定义范围
  <input type="checkbox" /> 复选框 
  <input type="file" />  文件上传组件
  <input type="button" /> 普通按钮
  <input type="submit"  name="注册"/> 提交按钮
  不写name属性否则将“提交”两个字提交到服务器
  <input type="reset" /> 重置按钮
  <input type="hidden" />  隐藏域

name：元素名，如果需要表单数据提交到服务器，必须提供name属性值，服务器通过属性值获得提交的数据。
value属性：设置input标签的默认值。submit和reset为按钮显示数据
size：大小
checked属性：单选框或复选框被选中。
readonly：是否只读
disabled：是否可用
maxlength：允许输入的最大长度
```

#### `下拉列表`:

```html
<select>
            <option></option>
</select>

<option>定义下拉选项
	name属性：发送给服务器的名称
	multiple属性：不写默认单选，取值为“multiple”表示多选。
	size属性：多选时，可见选项的数目。
<option> 子标签：下拉列表中的一个选项（一个条目）。
	selected ：勾选当前列表项
	value ：   发送给服务器的选项值。
```

#### 文本域

```html
<textarea></textarea>
	cols属性：文本域的列数
	rows属性：文本域的行数
```

#### div、span标签：

```html
<div></div>
div是HTML的一个普通标签进行区域划分
是块级标签独占一行
块级元素可以设置高宽

<span></span>
span是HTML的一个普通标签
共处一行，对行内元素进行美化
span用来对数据进行修饰
行内标签 共处一行
行内标签：默认不能设置高宽
```

div+css时页面布局最常用的方式。


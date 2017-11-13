---
layout: post
title:  "常见水平居中和垂直居中方法"
date:   2017-08-22 10:05:10
categories: 前端
tags: HTML CSS
author: 薛彬
---

* content
{:toc}

总结一下常见的水平居中和垂直居中的方法。。





## 水平居中

### 行内元素的居中

对于行内元素，居中简直不要太容易，父级元素中设置一个`text-align:center`即可。

![](http://omufjr5bv.bkt.clouddn.com/centering%E6%B0%B4%E5%B9%B31.png)

### 定宽块级元素的居中1

对于块级元素，如果知道它的宽度，居中起来也会很容易，比如这样：

```html
<div class="horizontal">
  <div class="demo demo-2">
    <div class="demo-2-item">我是块级元素</div>
  </div>
</div>
```

```css
.demo-2-item{
  width:200px;
  height:30px;
  background-color: lightblue;
  margin: 0 auto;
}
```

也就是设置了宽度之后，另它的`margin-left`和`margin-right`为`auto`。

![](http://omufjr5bv.bkt.clouddn.com/centering%E6%B0%B4%E5%B9%B32.png)

### 定宽块级元素的居中2

定宽块级元素还能这样：

```html
<div class="horizontal">
  <div class="demo demo-5">
    <div class="demo-5-item">我是要居中的那个</div>
  </div>
</div>
```
```css
.demo-5-item{
  position:absolute;
  width:200px;
  left:50%;
  margin-left:-100px;
  background-color: lightblue;
}
```

将它设为绝对定位，然后用`left:50%`将元素的最左侧移到绝对定位的正中间，再利用`margin-left：-100px`将它向左移动`100px`，也就是宽度的一半，就正好将元素的中线和容器的中线对齐了，实现居中效果。

![](http://omufjr5bv.bkt.clouddn.com/centering%E6%B0%B4%E5%B9%B35.png)

### 定宽块级元素的居中3

还有一种利用绝对定位的方式：

```html
<div class="horizontal">
  <div class="demo demo-6">
    <div class="demo-6-item">我是要居中的那个</div>
  </div>
</div>
```
```css
.demo-5-item{
  position:absolute;
  width:200px;
  left:0;
  right:0;
  margin:0 auto;
  background-color: lightblue;
}
```
#### left:0;right:0的作用

我们先看`left`和`right`两个属性，在绝对定位中，如果设置了`top: 0; left: 0; bottom: 0; right: 0` ，浏览器为它包裹一层新的盒子，这个元素会填满它相对父元素的内部空间。

假如我只设置了`left:0;right:0;`也就是这样：

![](http://omufjr5bv.bkt.clouddn.com/centering%E6%B0%B4%E5%B9%B361.png)

蓝色区域会充满整个父容器的宽度。

#### width:200px的作用

如果给元素设置了宽度，浏览器会阻止元素填满所有的空间。也就是这样：

![](http://omufjr5bv.bkt.clouddn.com/centering%E6%B0%B4%E5%B9%B362.png)

#### margin:0 auto的作用

根据盒子的计算规则，如果宽度是固定的，将`margin-left`和`margin-right`设为`auto`之后，这两个值会根绝父级元素的宽度自动计算除去元素的剩余宽度，然后均分，就实现了居中效果了。

![](http://omufjr5bv.bkt.clouddn.com/centering%E6%B0%B4%E5%B9%B36.png)

### flex

通过flex布局可以轻易实现居中，父级元素设置`display: flex;justify-content: center;`即可。

### CSS3：transform

使用CSS3中新增的transform属性, 子元素设置：

```
position:absolute;
left:50%;
transform:translate(-50%,0);
```

### grid

使用Grid来实现，父级元素设置:

```
display: grid;
grid-template-columns: 50px;
justify-content: center;
```

## 垂直居中

> 未完待续
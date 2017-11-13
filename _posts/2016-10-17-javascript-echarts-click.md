---
layout: post
title:  "Echarts3.x中click,datazoom,timeline事件的解决方法"
date:   2016-10-17 10:31:00
categories: 前端
tags: JavaScript Echarts
author: 薛彬
---

* content
{:toc}

在Echarts3中绑定事件是通过`on`方法，事件名称对应DOM事件名称，为小写的字符串。比如点击事件为：





```javascript
myChart.on('click', function (params) {
    alert("你点我干嘛？");
});
```

这里的`params`是图表的相关参数，可以在其中获取自己想要的数据。

再比如说图表中有`datazoom`组件，在滑动滑块后想获取动态数据，则可以这样：

```javascript
chart.on('datazoom', function(params) {
    var xAxis = chart.getModel().option.xAxis[0];
    var endTime = xAxis.data[xAxis.rangeEnd];
    fun(endTime)
});
function fun(date){
    console.log(date);
}
```

在这里虽然传递了`params`参数，但是里面我发现只有`start`和`end`的值，是一个百分数，不能利用它获取我们想要的最后一个数据，只能通过寻找当前x轴的最后一个刻度来获取滑动后的数据。

还有，`timeline`组件，这是一个时间轴的组件，也可以在点击时间后动态获取数据：

```javascript
chart.on('timelinechanged', function(params) {
    fun(params.currentIndex);
});
function fun(date){
    console.log(date);
}
```

`timelinechanged`中的`params`就比较友好，它提供了`currentIndex`这样的一个属性，可以获取到点击的这个时间在数组中的下标，我们就可以利用它来获取想要的数据了。

### 参数`params`

事件中包含的参数`params`是一个包含点击图形数据信息的对象，它的格式为：

```javascript
{
    // 当前点击的图形元素所属的组件名称，
    // 其值如 'series'、'markLine'、'markPoint'、'timeLine' 等。
    componentType: string,
    // 系列类型。值可能为：'line'、'bar'、'pie' 等。当 componentType 为 'series' 时有意义。
    seriesType: string,
    // 系列在传入的 option.series 中的 index。当 componentType 为 'series' 时有意义。
    seriesIndex: number,
    // 系列名称。当 componentType 为 'series' 时有意义。
    seriesName: string,
    // 数据名，类目名
    name: string,
    // 数据在传入的 data 数组中的 index
    dataIndex: number,
    // 传入的原始数据项
    data: Object,
    // sankey、graph 等图表同时含有 nodeData 和 edgeData 两种 data，
    // dataType 的值会是 'node' 或者 'edge'，表示当前点击在 node 还是 edge 上。
    // 其他大部分图表中只有一种 data，dataType 无意义。
    dataType: string,
    // 传入的数据值
    value: number|Array
    // 数据图形的颜色。当 componentType 为 'series' 时有意义。
    color: string
}
```

在点击后，我们可以很容易的通过形如`params.name`的调用方式获取想要的数据，以便于数据传递。





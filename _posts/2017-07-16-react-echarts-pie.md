---
layout: post
title:  "React简单封装ECharts饼图"
date:   2017-07-16 16:00:54
categories: 前端
tags: React
author: 薛彬
---

* content
{:toc}


今天简单的试了试用React来封装一个Echarts图例。




## 使用ECharts

### 获取ECharts

Echarts官网教程中就有告诉通过各种方式获取Echarts：[http://echarts.baidu.com/tutorial.html](http://echarts.baidu.com/tutorial.html "获取Echarts")

最简单的方式就是直接引用js文件就o了~

当然，这次要用的是webpack来进行包管理，所以就要通过webpack来获取Echarts：

```
npm install echarts --save
```

### 引入ECharts

通过webpack获取的ECharts会放在项目的`node_modules`目录下，可以直接通过`require('echarts')`得到。

> 默认使用 require('echarts') 得到的是已经加载了所有图表和组件的 ECharts 包，因此体积会比较大，如果在项目中对体积要求比较苛刻，也可以只按需引入需要的模块。

官方式这么说的，所以我们要到[https://github.com/ecomfe/echarts/blob/master/index.js](https://github.com/ecomfe/echarts/blob/master/index.js "https://github.com/ecomfe/echarts/blob/master/index.js")去查看可以引入的模块列表，并按需引入。

比如这次我需要画一个饼图，我就需要引入：

```javascript
var echarts = require('echarts/lib/echarts')
require('echarts/lib/chart/pie')
require('echarts/lib/component/title')
```

## gogogo

### 定义组件

我们先定义一个`Component`,ECharts的渲染是需要在这个div有宽高的前提下，所以在这个组件的`render()`方法中要有一个div容器来展示ECharts,并且这个div要设置宽高。

```javascript
render() {
	return (
    <div className="pie-react">
      <div ref="pieChart" style={{width: "200px", height: "200px"}}>
      </div>
    </div>
  )
}
```

### initPieChart()

就像平时使用ECharts一样，我们先是获取数据，然后`init`ECharts，然后`setOption`，就搞定了~

```javascript
initPieChart() {
  const { data } = this.props
  let myChart = echarts.init(this.refs.pieChart)
  let options = this.setOption(data)
  myChart.setOption(options)
}
//这是一个最简单的饼图~
setOption(data) {
  return {
    title:{
      text:"编程语言",
      left:"center"
    },
    series : [
      {
        name: '比例',
        type: 'pie',
        data: data,
        label: {
          normal: {
            formatter: "{d}% \n{b}",
          }
        }
      }
    ]
  }
}
```

### 绑定方法

```javascript
constructor(props) {
  super(props)
  this.setOption = this.setOption.bind(this)
  this.initChart = this.initChart.bind(this)
}
```

### 初始化ECarts

上面已经定义了`initPieChart`方法，我们该在什么时候用这个方法呢？

当然是要在DOM渲染完成了之后啊，然后通过refs去获取这个DOM节点~

也就是`componentDidMount`的时候：

```javascript
componentDidMount() {
  this.iniChart();
}
```

### 更新

```javascript
componentDidUpdate() {
  this.iniChart();
}
```

### 传数据

```javascript
import Pie from './pie';
const data = [
  {value: 2, name: "JavaScript"},
  {value: 1, name: "Java"},
  {value: 1, name: "HTML/CSS"}
]
export default class ComponentBody extends React.Component{
  render(){
    return(
      <div className="bodyindex">
        <Pie data={data}/>
      </div>
    )
  }
}
```

### 结果

![](http://i.imgur.com/OvOLAnh.png)

完整代码见[https://github.com/axuebin/react-echarts-demo/blob/master/src/pie.js](https://github.com/axuebin/react-echarts-demo/blob/master/src/pie.js "https://github.com/axuebin/react-echarts-demo/blob/master/src/pie.js")
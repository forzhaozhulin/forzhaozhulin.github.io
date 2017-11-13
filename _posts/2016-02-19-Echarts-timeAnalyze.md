---
layout: post
title:  "使用Echarts图表展示数据--时间比例分析"
date:   2016-02-18 19:14:54
categories: 前端
tags: JavaScript
author: 薛彬
---

* content
{:toc}

从2月1号开始到今天这快20天的时间，每天粗略的记录了一下每个行为的时间，也就有了一份小数据，我用Echarts图表来将这些数据可视化展示出来。





## 关于Echarts

ECharts，一个纯 Javascript 的图表库，可以流畅的运行在 PC 和移动设备上，兼容当前绝大部分浏览器，底层依赖轻量级的 Canvas 类库 ZRender，提供直观，生动，可交互，可高度个性化定制的数据可视化图表。

## 使用Echarts

### 新建html

在html页面放入一个div

```html
<div class="charts" id="chart1" style="width:30%;float:left;margin-right:0;padding-right:0;border-right-width:0">
    <script>
        display_timeAnalyze();
    </script>
</div>
```

这样就建立了一个div，在这个div中会调用`display_timeAnalyze()`。

### 新建js

js文件中才是Echarts的关键，其实Echarts使用起来相当简单，它的API非常的详细，而且提供各式各样的实例。

```javascript
function display_timeAnalyze() {
    $('#chart1').css({
        height: $(window).height() * 0.5,
        width: 600
    });
    require.config({
        paths: {
            echarts: './scripts/echarts/build/dist/'
        }
    });
    // 使用
    require(
            [
                'echarts',
                'echarts/chart/pie', // 使用饼状图就加载pie模块，按需加载
                'echarts/chart/funnel'
            ],
            function (ec) {
                var pie_chart = ec.init(document.getElementById('chart1'));
                pie_option1 = {
                    title: {
                        text: '2月时间占用比例（每天24小时）',
                        x: 'center',
                        y: 'top'
                    },
                    legend: {
                        orient: 'vertical',
                        x: 'right',
                        data: ['学习', '睡觉', '吃饭', '上课', '上网', '娱乐', '运动', '其它']
                    },
                    series: [
                        {
                            type: 'pie',
                            radius: ['30%', '60%'],
                            data: [
                                {value: 2.39, name: '学习'},
                                {value: 9.56, name: '睡觉'},
                                {value: 1.22, name: '吃饭'},
                                {value: 0, name: '上课'},
                                {value: 3.67, name: '上网'},
                                {value: 5.61, name: '娱乐'},
                                {value: 0, name: '运动'},
                                {value: 1.56, name: '其它'},
                            ],
                            itemStyle: {
                                normal: {
                                    label: {
                                        show: true,
                                        formatter: '{b}:{d}%',
                                        textStyle: {
                                            fontFamily: '微软雅黑',
                                            fontSize: '12'
                                        }
                                    }
                                }
                            }
                        }
                    ]
                };
                pie_chart.setOption(pie_option1);
            }
    );
}
```

由于数据比较少，我选择了直接输入的方式。如果数据量比较大，我们可以采取载入csv文件转成数组，或是存入数据库等等的形式。

这是最后的结果：

![](http://i.imgur.com/78YZgyJ.png)

还听好看的~

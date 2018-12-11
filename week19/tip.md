## ARTS - Tip
## 补11.12
## ECharts 的使用简介
项目中使用了ECharts， 主要说下echarts的 dataset 和 dataZoom.

### dataset

在option里设置dataset就是设置数据源

```
dataset: {
            // 这里指定了维度名的顺序，从而可以利用默认的维度到坐标轴的映射。
            // 如果不指定 dimensions，也可以通过指定 series.encode 完成映射，参见后文。
            dimensions: ['legendName', 'col2', 'col3'],
            source: result
        },
```
这里result是数据数组，用来提供需要展示的数据，这样比以下直接分割写数据更紧凑些。

```
legend: {
                data:['销量']
            },
            xAxis: {
                data: ["衬衫","羊毛衫","雪纺衫","裤子","高跟鞋","袜子"]
            },
            yAxis: {},
            series: [{
                name: '销量',
                type: 'bar',
                data: [5, 20, 36, 10, 10, 20]
            }]
```

### dataZoom

这个是底下下拉栏。数据很多时候，需要左右拖动来调整显示，就用到了dataZoom.

写法如下：

```
dataZoom: [
            {
                type: 'slider',
                show: true,
                xAxisIndex: [0],
                left: '9%',
                bottom: -5,
                startValue: 0,
                endValue: 10
            }
        ],

```
注意这里的startValue和endValue, 这个控制图表显示的起始阶段和容量，如果需要搜索某一个特定数据时候，找到该数据在列表中的坐标，直接设置startValue 就可以把该数据放在第一位，比再请求数据更方便高效些。
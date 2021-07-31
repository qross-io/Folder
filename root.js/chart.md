# 图表组件

CHART 标签封装了 echarts.js，以简化图表的呈现。原有的 echarts 设置非常复杂，即使一个简单的折线图，也要写很多行 Json 配置代码，CHART 标签把 echarts 的各种设置变化为了标签和属性。CHART 标签目前只支持单个图表的呈现，更多功能会慢慢加入进来。

```html
<model name="NodeStat" data="/api/keeper/monitor"></model>
<chart id="BusyScores" title="负载评分统计" type="line" x-axis="@NodeStat#moment" y-axis="@NodeStat#busy_scores"></chart>
<chart id="CpuUsage" title="CPU 使用率统计" type="bar" x-axis="@NodeStat#moment" y-axis="@NodeStat#cpu_usage"></chart>
```

上例中，[MODEL 标签](/root.js/model.md)用于从后端加载数据，接口返回一个包含多列的一个二维表格。两个图表都使用 MODEL 中的数据，其他两个图表的 X 轴均使用 MODEL 中的`moment`列的数据，第一个图表使用`busy_scores`列的数据，第二个图表使用`cpu_usage`列的数据。

## CHART 标签属性

CHART 标签支持的属性如下，简单设置即可呈现为图表。

* `data` 除了可以从 MODEL 加载数据外，CHART 自己也可以从后端加载数据。详见[与数据相关的属性](/root.js/data.md)。
* `title` 图表的标题。
* `type` 图表的类型，理论上支持 echarts 所有类型的图表，如`line`、`bar`、`pie`等。
* `x-axis` 设置 X 轴的数据。支持提取从 MODEL 中或从 CHART 的`data`属性获取的数据。属性值设置见下面的说明。
* `y-axis` 设置 Y 轴的数据，设置同`x-axis`。
* `width` 图表的宽度，默认值`100%`。图表会显示在一个 DIV 中，这个属性用来设置 DIV 的宽度。
* `height` 图表的调试，默认值`300`，单位像素。这个属性用来设置图表所在 DIV 的高度。

其中`x-axis`和`y-axis`属性支持从 MODEL 或标签本身的`data`中获取数据。从 MODEL 中获取数据的规则见本文第一个示例，例如`@NodeStat#cpu_usage`用来获取 MODEL 返回的二维表格中一整列，其中`NodeStat`为 MODEL 的名字，`cpu_usage`为表格中的列名。获取 CHART 标签的数据直接使用`#cpu_usage`即可，占位符规则与 [TEMPLATE 标签](/root.js/template.md)相同。假如`data`属性返回的结果如下：

```json
{
    
    "title": "Grade 6 Scores",
    "data": [
        {
            "name": "Tom",
            "score": 87
        },
        {
            "name": "Jerry",
            "score": 76
        },
        ...
    ]
}
```

可使用`@:data#name`获取所有的学生姓名，使用`@:data#score`获取所有的学生成绩。

## CHART 方法和事件

可用事件有两个，可用于跟其他组件交互。

* `onload` 当图表第一次加载完成时触发。
* `onreload` 当图表重新加载完成时触发。

可用方法只有一个`reload`，即重新加载图表。另外，如果图表中使用了 MODEL 的数据，那么当 MODEL 数据变化时，CHART 会自动触发`reload`事件以重新呈现图表。

---
参考链接

* [Model 数据加载模型](/root.js/model.md)
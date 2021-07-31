# Log 日志组件

Log 日志组件可以说是专门为 Keeper 中的日志回显开发的功能。Keeper 中有运行日志和任务日志，LOG 组件可以实时调取日志并显示在页面上，并支持彩色日志等功能。

```html
<model name="running" auto-refresh="yes" interval="3000" data="/api/keeper/running?node_address=<%=$node.node_address%>&start_time=<%=$start_time%>"></model>
<log id="Logs" data="/api/keeper/running-logs?node_address=<%=$node.node_address%>&cursor=$:[cursor]&hour=$:[hour]" terminal="@running > 0" auto-start="no" delay="120" cursor="-1" filter="$(#Filter)" hour="<%=@now FORMAT 'yyyyMMdd/HH'%>"></log>
```

先介绍一下 Log 组件相关的属性：

* `data` 从后端获取日志数据，返回值的格式有限制，下面会介绍。
* `auto-start` 是否自动开始加载。
* `terminal` 终止加载的条件，是一个 Javascript 表达式。
* `interval` 加载间隔时间，单位毫秒，默认值`1000`，即`1`秒刷新一次。
* `load-until-empty` 加载到没有日志时即停止，默认为`true`。满足`terminal`时还要满足这个条件时才终止。
* `delay` 在结束加载时还要延长加载几次，默认为`3`，有可能还有日志数据没有被完全加载出来。
* `cursor` 日志文件的光标位置，在下文会说明这个属性的作用。
* `filter` 日志过滤关键词，只有满足条件的日志才显示。

Log 组件相关的事件如下：

* `onload` 第一加载完成后触发。加载完成表示满足中止条件且达到`delay`设置的次数。
* `onreload` 重新加载完成后触发。重新加载会清空所有已经显示的日志。
* `onterminate` 达到终止条件时触发。

## 后端获取日志的逻辑

Keeper 的日志都存储在文本文件中，Keeper 运行日志每小时生成一个文件，任务日志是每次执行生成一个文件。因为每个文件都比较大，所以一次性完成加载不现实，只能分多次读取。Keeper 提供的接口是每次加载 200 行。

因为是多次读取，所以每次读取后都要记录这次读取到哪里，使用“指针”来保存位置值，即属性中的`cursor`，每次读取后都会自动更新`cursor`属性的值。当下次读取时，把最新的值再传递给后端。`cursor`属性一般使用两个初始值，`0`表示从文件开头即第一行开始读取，`-1`表示从文件末尾即最后一行开始读取。

后端返回的数据格式为：

```json
{
    "logs": [
        {
            "datetime": "2021-07-28 23:32:00",
            "logType": "DEBUG",
            "logText": "Keeper Beat!"
        }
    ],
    "cursor": 2834
}
```

其中各字段的名称是固定的，不可修改，也不可设置。

## 前端显示日志的逻辑

由于后端是多次加载的，所有前端一直也是多次将日志显示出来的。所以 LOG 组件永远是自动刷新的，且每`interval`秒刷新一次，每次刷新时都会判断上一次的加载是否完成，如果没有完成那么这次就会取消刷新，直到下一次刷新时再次判断。

这个示例来自于重启 Keeper 功能，简单说明一下逻辑。

* 默认不自动加载，即`auto-start`为`false`，由重启按钮触发加载。`<button id="RestartButton" onclick+success-="start: #Logs">`
* 从文件末尾开始加载，即`cursor="-1"`。
* Model 组件每`3`秒刷新一次，从后端获取状态。Log 组件每次刷新都根据 Model 的值判断是否达到中止条件，即`terminal="@running > 0"`。
* 达到中止条件后依然再刷新`120`次，即`delay="120"`。
* 后端返回值中，除了`logs`和`cursor`外的字段会自动赋值给 LOG 标签。示例中还返回`hour`字段，将返回值自动赋值给 LOG 标签的`hour`属性，下次获取数据时自动传递这个值，即`data`属性中的`hour=$:[hour]`。
* `filter`属性用于对日志进行筛选，只有满足条件的日志才会显示出来。由于性能问题，日志筛选在前端实现，这里支持正则表达式，且不区分大小写。`filter`的属性指向文件框`#Filter`，用于获取用户输入。
* 前端显示日志会自动将其中的一些信息转为彩色，如日期时间、数字等。


---
参考链接

* [Model 数据加载模型](/root.js/model.md)
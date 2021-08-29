# IF 标签

IF 标签也是 [Model 数据加载模型](/root.js/model.md)的一部分，引用文件与 Model 相同。IF 标签用于逻辑判断，逻辑成立时才显示标签中的内容。**IF 标签只执行一次，呈现内容后会删除自己**。

```html
<if test="boolean expression" onload="event" onreturntrue="event" onreturnfalse="event">
    ...html code
<else-if test="boolean expression">
    ...html code
<else>
    ...html code
</if>
```

* `test` 必须有，没有则默认结果为`false`。这本质是一个 Javascript 表达式，可使用 Model 的[数据占位符](/root.js/holder.md)数据和 [Express 字符串](/data/express.md)支持的占位符。
* `else-if` 标签可以有多个，。
* `else` 标签只能有一个，如果设置了多个，则只有第一个生效。
* `onload` 事件，加载完成后执行，无论条件结果为`true`还是`false`。事件只能写在标签上。
* `onreturntrue` 事件，所有 IF 或 ELSIF 条件有一条为`true`时触发。事件只能写在标签上。
* `onreturnfalse` 事件，所有 IF 或 ELSIF 条件都为`false`时触发。事件只能写在标签上。

```html
<for in="select * from students">
    @[name]: 
    <if test="@[score] > 90">
        Excellent!
    <else-if test="@[score] > 75">
        Good Job!
    <elsif test="@[score] > 60">
        Keep studing.
    <else>
        Are you OK?
    </if>
</for>

<if test="'$(#key)' != ''">
    ...
<else-if test="&(score) > 1">
    ...
<else-if test="@model.students < 10">
    ...
</if>
```

IF 标签内的 HTML 内容支持[嵌入式 Javascript 表达式](/root.js/express.md)。

## IF 属性

除了 IF 标签外，Model 库提供了`if`属性，用于决定是否呈现元素。与 VUE 中的`v-if`属性类似。

```html
<div if="5 > 2">Hello</div>
<div else-if="5 < 7">World</div>
<div else>N/A</div>
```

`if`属性也支持`onload`、`onreturntrue`和`onreturnfalse`事件，定义在`if`属性所在元素上。`if`和`else-if`属性值支持 Model 数据占位符和 [Express 字符串](/data/express.md)支持的占位符。`if`、`else-if`和`else`如果一同使用，则需要设置同级元素上才能识别为同一组逻辑。不满足条件的元素将会被移除。

SELECT 标签的 OPTION 标签和 TABLE 标签的 TR 标签不仅不支持 FOR 标签作为它们的父级，也不支持 IF 标签，这种情况下可以通过设置`if`属性来确定是否呈现，如：

```html
<table  data="...">
    <template>
        <tr if="@[age] >= 18">
            ...
        </tr>
    </template>
</table>
```

包含 if 属性的标签的 HTML 内容 **不支持** [嵌入式 Javascript 表达式](/root.js/express.md)。


---
参考链接

* [Model 数据加载模型](/root.js/model.md)
* [布尔属性](/root.js/boolean.md)
* [Express 字符串](/root.js/express.md)
* [数据占位符](/root.js/holder.md)
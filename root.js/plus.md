# 增强属性

“增强属性”是对原生属性的扩展，原生属性是一个具体的值，而增强属性是一个表达式，通过自动计算再将计算结果赋值给对应的原生属性，可以理解为增强属性是原生属性的赋值表达式。区别于原生属性，增强属性在属性名称后多一个加号`+`，表示“增强”、“扩展”的意思。例如`value`属性对应增强属已为`value+`。

```html
<select value+="@students.grade">
    <option value="1">Grade 1</option>
    <option value="2">Grade 2</option>
    <option value="3">Grade 3</option>
</select>
<span data>$(#Input1):$(#Input2)</span>
<input type="text" value+="~{ @student.age > 17 ? 'Adult' : 'Nonage' }" />
<a href+="/job/detail?id=&(jobId)">Job Detail</a>
```

增强属性本质上是一个赋值表达式，赋值表达式在得到结果时经过三步运算：

1. 从 [Model 模型](/root.js/model.md)（有时也从 Template 中）中获取数据并替换。
2. 进行 [Express 字符串](/root.js/express.md)运算。
3. 将前两步的运算结果赋值给对应的原生属性。

除了带加号`+`的属性外，还有两种类型的属性其他本质上也是增强属性。一种是[与数据相关的属性](/root.js/data.md)，如`data`和`in`；另一种是[布尔属性](/root.js/boolean.md)，如`test`和`terminal`等。

```html
<model name="students" data="select grade, count(0) as amount from students -> first row">
<if test="@students.amount > 100">
    <input value+="@students.amount" />
</if>
<log id="Logs" terminal="@running > 0"></log>
```

因为增强属性在加载时进行一次 Express 字符串运算，Express 运算在 Model 数据运算之后。所以即时没有加载 Model 数据，也可以单独进行 Express 运算。可以理解是对属性值的扩展。

```html
<a href+="/job/detail?id=&(jobId)">Job Detail</a>
```

上例会自动解析`href+`属性将值解析后替换为`href`属性，解析后可以是：

```html
<a href="/job/detail?id=3">Job Detail</a>
```

---
参考链接

* [Model 数据加载模型](/root.js/model.md)
* [Express 字符串](/root.js/express.md)
* [与数据相关的属性](/root.js/data.md)
* [布尔属性](/root.js/boolean.md)


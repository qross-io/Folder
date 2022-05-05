# SPAN 标签扩展

SPAN 标签也属于 [Model 数据加载模型](/root.js/model.md)的一部分，引用文件也与 Model 相同。在 Model 模型中，[O 标签](/root.js/o.md)不是原生标签，没有样式控制及更多功能，为了实现更灵活的数据定制功能，标签库库扩展了 SPAN 标签，也可用于输出数据。

## 加载数据

```html
<span onload="event" onreload="event" await="" reload-on="click:#Button1" data="SELECT title, views FROM table1 WHERE id=$id -> FIRST ROW">文章“@:title”的阅读数是 @:views!!</span>
```

SPAN 扩展标签需要设置`data`属性才能够处理数据，使用一些其他特性如事件表达式也需要设置`data`属性。

* `await` 如果要等待其他的数据相关组件加载完成之后再加载则需要设置这个属性，如`await="#Select1"`表示等`#Select1`加载完成之后再加载。如果要等待多个组件，那么组件 ID 之间使用逗号分隔即可。
* `onload` 第一次数据加载完成后触发的事件。
* `onreload` 重新加载数据后触发的事件。
* `reload-on` 指定重新加载数据的条件，由其他组件触发。规则详情[事件表达式](/root.js/event.md)。

SPAN 标签的事件只能写在标签上，SPAN 扩展标签不提供扩展标签的选择器。SPAN 标签可以使用其他原生属性来做其他操作，比如控制样式。

SPAN 标签的占位符语法规则为 `@:keyOrPath?(defaultValue)`，与 [O 标签](/root.js/o.md)相同。更详细说明请参见[数据占位符](/root.js/holder.md)。

上例中，`data`属性返回一个有两个字段的对象，如可以是`{ "title": "低代码平台简单介绍", "views": 209 }`。`@:title`可以获取`低代码平台简单介绍`，`@:views`可以获取到`209`，最后 SPAN 标题中展示`文章“低代码平台简单介绍”的阅读数是 209!`。`@:views`后面有两个叹号，前一叹号表示占位符结尾，防止字符冲突。可使用`@:/`获取`data`属性加载的所有数据。

SPAN 标签也可以从 MODEL 中获取数据，这时无需要设置`data`属性的值，但一定要有`data`属性。例如：

SPAN 标签也支持 Javascript 短句占位符，详见 [MODEL 标签](/root.js/model.md)中的说明。

```html
<span data>文章“@page.title”的阅读数是 @page.views!!</span>
```

因为 SPAN 标签支持重新加载，所以当 MODEL 的数据更新时，SPAN 标签的数据也会同时更新。

## 复制内容

SPAN 标签新增扩展方法`copy()`，执行后可复制 SPAN 元素内的数据。

```html
<span copy-on="click: #CopyButton" data>Hello World.</span> <!--data属性必须有才能支持`copy-on`-->
<a id="CopyButton"><i class="iconfont icon-file-copy"></i></a>
```

或

```html
<span id="Hello" copy-text="文字已复制。" data>Hello World.</span> <!--data属性可以没有-->
<a onclick-="copy: #Hello"><i class="iconfont icon-file-copy"></i></a>
```

可以通过`copy-text`标签属性设置复制成功后的提示文字。

---
参考链接

* [Model 数据加载模型](/root.js/model.md)
* [O 标签](/root.js/o.md)
* [数据占位符](/root.js/holder.md)
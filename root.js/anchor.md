# A 链接扩展

对 A 标签的扩展主要是加上的[服务器端事件](/root.js/server.md) `onclick+`的支持，可以理解为是 [Button 按钮扩展](/root.js/button.md)的精简版。

```html
<a onclick+="delete:/api/test?id=&(id)" confirm-text="确认要删除吗？" confirm-title="删除确认" failure-text="删除失败！" exception-text="发生错误：{data}" callout="upside" href="/link-to-other-path">删除</a>
```

新增的扩展属性有：

* `onclick+` 当点击时触发的[服务器端事件](/root.js/server.md)，是 A 标签扩展的关键属性。
* `confirm-text` 点击链接后的确认对话框文字内容，点击确认后才会执行服务器端事件。
* `confirm-title` 设置确认对话框的标题文字，对于原生确认对话框无效。
* `confirm-button-text` 设置弹出对话框确认按钮的文字，默认为`OK`，对于原生确认对话框无效。
* `cancel-button-text` 设置弹出对话框取消按钮的文字，默认为`Cancel`，对于原生确认对话框无效。
* `success-text` 请求成功后的提示文字，本文下方有状态说明。
* `failure-text` 请求失败后的提示文字，本文下方有状态说明。
* `exception-text` 请求发生异常后的提示文字，替换按钮上的文字，“异常”是指接口或 PQL 语句发生错误，错误会被打印到控制台。
* `callout` 如果不设置`alert`属性，则默认使用 Callout 来提示以上各种消息。这个属性可以设置 Callout 的位置，默认为`upside`。
* `alert` 如果设置了这个属性，则使用 Alert 弹出框来提示各种消息，Callout 设置则无效。

以上所有`-text`结尾的消息属性均支持 [Express 字符串](/root.js/express.md)。

另外，如果设置了`href`属性，会在服务器端事件执行完成后，且执行结果为`success`状态，则会跳转到对应的地址。但是对`target="_blank"`支持不好，会转成`window.open`弹出窗口方式，可能会被浏览器拦截，不过这种场景应用比较少。

---
参考链接

* [服务器端事件](/root.js/server.md) 
* [Express 字符串](/root.js/express.md)
* [Button 按钮扩展](/root.js/button.md)
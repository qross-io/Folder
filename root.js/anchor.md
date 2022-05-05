# A 链接扩展

对 A 标签的扩展主要是加上的[服务器端事件](/root.js/server.md) `onclick+`的支持，可以理解为是 [Button 按钮扩展](/root.js/button.md)的精简版。

```html
<a onclick+="delete:/api/test?id=&(id)" confirm-text="确认要删除吗？" confirm-title="删除确认" failure-text="删除失败！" exception-text="发生错误：{data}" callout="upside" href="/link-to-other-path">删除</a>
```

A 标签扩展对应的 Javascript 文件为 `root.anchor.js`。

A 标签扩展新增的扩展属性有：

* `callout`或`callout-position` 默认使用 [Callout](/root.js/root.md) 来提示以上各种消息。这个属性可以设置 Callout 的位置，默认为`upside`。
* `message`或`message-duration` 如果设置了这个属性，则使用 Message 提示框（在页面上方弹出）来提示各种消息，属性值为持续时间，单位为秒，`0`表示一直显示直到点击才关闭。
* `confirm-text` 点击链接后的确认对话框文字内容，点击确认后才会执行服务器端事件。
* `confirm-title` 设置确认对话框的标题文字，对于原生确认对话框无效。
* `confirm-button-text` 设置弹出对话框确认按钮的文字，默认为`OK`，对于原生确认对话框无效。
* `cancel-button-text` 设置弹出对话框取消按钮的文字，默认为`Cancel`，对于原生确认对话框无效。
* `success-text` 请求成功后的提示文字，本文下方有状态说明。
* `failure-text` 请求失败后的提示文字，本文下方有状态说明。
* `exception-text` 请求发生异常后的提示文字，替换按钮上的文字，“异常”是指接口或 PQL 语句发生错误，错误会被打印到控制台。

以上所有`-text`结尾的消息属性均支持 [Express 字符串](/root.js/express.md)。

A 标签没有对方法进行扩展，只有一个通用方法`set(attr, value)`，可以用来设置 A 标签的各种属性。

扩展事件主要是`onclick+`：

* `onclick+` 当点击时触发的[服务器端事件](/root.js/server.md)，是 A 标签扩展的关键事件。
* `onclick+success` 客户端事件，当`onclick+`执行完成且结果符合预期时触发。
* `onclick+failure` 客户端事件，当`onclick+`执行完成但是结果不符合预期时触发。
* `onclick+exception` 客户端事件，当`onclick+`执行出错时触发。
* `onclick+completion` 客户端事件，当`onclick+`执行完成时触发，无论结果如何。

在有`onclick+`事件时，`href`属性同样也支持，可以处理服务器端返回的数据。

```html
<a onclick+="post:/api/page?title=$(#Title)" href="/page/detail?id={data.id}">Add Page</a>
<a class="delete" onclick+="delete:/api/user?id=&(userid)" confirm-text="确定要删除这个用户吗？" confirm-button-text="确定" cancel-button-text="取消" confirm-title="确认删除" href="/user/users">删除用户</a>
```

如果设置了`href`属性，会在服务器端事件执行完成后，且执行结果为`success`状态，则会跳转到对应的地址，如上例。但是对`target="_blank"`支持不好，会转成`window.open`弹出窗口方式，可能会被浏览器拦截，不过这种场景应用比较少。


---
参考链接

* [服务器端事件](/root.js/server.md) 
* [Express 字符串](/root.js/express.md)
* [Button 按钮扩展](/root.js/button.md)
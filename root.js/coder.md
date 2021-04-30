# 彩色编码 Coder

**Coder 组件** 是基于 [CodeMirror](https://codemirror.net/) 的进一步封装，实现彩色编码功能，但是不再使用 Javascript 的初始化方式并增了一些自动功能。

```html
<textarea coder="java"></textarea>
```

Coder 以 **TEXTAREA** 作为配置标签，并支持所有 CodeMirror 的原有属性。

```html
<textarea coder="pql" rows="4" readonly="false" line-wrapping="true" line-numbers="true"></textarea>
```

在设置属性时需要使用分隔符格式，如上例中使用`line-wrapping`而不是`lineWrapping`。Coder 常用属性简单列表如下：

* `value` 获取代码框中的内容，这是最关键属性。设置值即设置 TEXTAREA 标签的值即可。
* `mode` 语言，默认`pql`。这里可使用 mineType 值，也可直接使用语言名称，如`java`。
* `readOnly` 是否只读，默认`false`。
* `rows` 最小行数，默认`1`。
* `save` 保存代码的动作，支持接口和 PQL 语句，详见[与数据相关的属性](/root.js/data.md)。当按下`Ctrl`+`Enter`时触发保存动作。例如：
    ```html
    <textarea coder="html" save="/api/code/update?id=&(id)&code="></textarea>
    <textarea coder="json" save="/api/data/add?code={value}%&format=json"></textarea>
    ```
* `validator` 验证器，仅`coder`或`mode`属性为`json`时可用，支持`array`、`object`、`object-array`三个值，分别验证数组、对象和对象数组。当格式不正确时，提示错误信息或`invalid-text`的内容。如下例：
    ```html
    <textarea coder="json" validator="object"></textarea>
    ```
* `save-text` 保存提醒文字。
* `saving-text` 保存中提醒文字。
* `saved-text` 保存完成提醒文字。
* `required-text` 代码框为空时的提示文字。
* `invalid-text` 代码格式不正确时的提示文字，仅支持 Json 的验证。
* `valid-text` 代码格式正确时的提示文字。
* `success-text` 保存成功时的提示文字，默认与`saved-text`相同。
* `failure-text` 后端验证失败时的提示文字。
* `exception-text` 后端事件发生错误时的提示文字。

以上`-text`结尾的属性显示在代码框的右下角。

与方法相关的属性有 3 个，属性值均为[精简事件和事件表达式](/root.js/event.md)。

* `save-on` 当其他标签或元素行为触发的保存。
* `clear-on` 当其他标签或元素行为触发的清空。
* `copy-on` 当其他标签或元素行为触发的将所有代码复制到剪切板。

以上 3 个属性对应的方法分别为`save()`、`clear()`和`copy()`。

事件主要有`onsave`，当保存代码内容时触发，支持`return`语句，当`return false`时中断保存。保存事件在输入框失去焦点或者同时按下`Ctrl+Enter`时触发。

```javascript
$s('#Coder1').onsave = function() {
    //this.value;
    return false;
}
```

还有一个与之对应的[服务器端事件](/root.js/server.md)`onsave+`。

```html
<textarea coder="json" onsave+="put:/api/save-code?id=${id}&content="></textarea>
```

另外两个事件不常用，`onfocus`和`onblur`，当输入框获得焦点和失去焦点时触发。

---
参考链接

* [CodeMirror 官网](https://codemirror.net/)
* [与数据相关的属性](/root.js/data.md)
* [服务器端事件](/root.js/server.md)
# 彩色编码 Coder

**Coder 组件** 是基于 [CodeMirror](https://codemirror.net/) 的进一步封装，实现彩色编码功能，但是不再使用 Javascript 的初始化方式并增了一些自动功能。

```html
<textarea coder="yes" mode="java"></textarea>
```

Coder 以 **TEXTAREA** 作为配置标签，并支持所有 CodeMirror 的原有属性。

```html
<textarea coder="yes" mode="pql" rows="4" read-only="false" line-wrapping="true" line-numbers="true"></textarea>
```

在设置属性时需要使用分隔符格式，如上例`line-wrapping`而不是`lineWrapping`。Coder 常用属性简单列表如下：

* `value` 获取或设置代码框中的内容，这是最关键属性。
* `readOnly` 是否只读，默认`false`
* `rows` 最小行数，默认`1`
* `mode` 语言，默认`pql`。这里可使用 mineType 值，也可直接使用语言名称，如`java`
* `action` 保存代码的动作，支持接口和 PQL 语句，详见 [Expression 表达式](/root.js/express.md)
* `saveText` 保存提醒文字。
* `savingText` 保存中提醒文字。
* `savedText` 保存完成提醒文字。

事件只有一个：

`onsave` 当保存代码内容时触发。

---
参考链接

* [CodeMirror 官网](https://codemirror.net/)
* [Express 字符串](/root.js/express.md)
# INPUT 标签扩展

为了更方便的处理表单中的用户输入，组件库对 INPUT 标签进行了扩展。

```html
<input type="text" name="Input1" required-text="" invalid-text="" valid-text="" validator="" minlength="0" class="" focus-class="" warn-class="" valid-class="" warn-text-class="" valid-text-class="" get="" set="" prevent-injection  />
```

* `type` 文本框的类型，现在只有`text`类型。
* `required-text` 当输入值为空时的提示文字，显示在输入框的右方。
* `invalid-text` 当输入值不正确时的提示文字，显示在输入框的右方。
* `valid-text` 当输入值正确时的提示文字，显示在输入框的右方，不设置则不显示。
* `validator` 正则表达式验证器，如`^[a-z]$`只允许输入小写英文字母，可以使用 Javascript 的格式，如`/^[a-z]$/i`来忽略大小写。
* `minlength` 最小字符长度，当不满足要求时提示`invalid-text`。小于等于`0`表示不限制最小长度。这个属性是原生属性。
* `class` 默认的显示样式，必填项默认值为`input-required-class`，选填项默认值为`input-optional-class`。
* `focus-class` 当文本框获得焦点时的样式，默认使用系统样式。
* `warn-class` 当输入不正确时的样式，默认使用系统样式。
* `valid-class` 当输入正确时的样式，默认使用系统样式。
* `warn-text-class` 警告文字的样式，默认为红色。
* `valid-text-class` 输入正确时的提醒文字的样式，默认为绿色，需设置`valid-text`才显示。
* `get` 在获取输入值时对值进行加工，返回加工后的值，下面有详细的解释。
* `set` 在设置输入值时对值进行加工，设置输入值为加工后的值，下面有详细的解释。
* `prevent-injection` 自动在获取值时将单引号`'`转成两个单引号`''`以防止 SQL 注入。需使用扩展选择器，例如`$s('@TextBox1`)`。

## 示例

使用时需要引入：

```html
<script type="text/javascript" src="/root.js"></script>
<script type="text/javascript" src="/root.input.js"></script>
```

<script type="text/javascript" src="@/root.input.js"></script>

以下文本框只能输入英文字符：

 <input type="text" required-text="这个字段不能为空。" invalid-text="只能输入英文字符，且最少输入3个。" valid-text="输入正确。" validator="^[a-zA-Z]+$" minlength="3" size="40" />

示例代码为：

 ```html
<input type="text" required-text="这个字段不能为空。" invalid-text="只能输入英文字符，且最少输入3个。" valid-text="输入正确。" validator="^[a-zA-Z]+$" minlength="3" size="40" />
 ```

## 用`get`和`set`属性对输入值进行拦截

`get`和`set`实现拦截器的功能，当获取或设置`value`属性的值时，对值进行加工，并返回加工后的值。这两个属性的属性值是一个 Javascript 短句，因为输入值是一个字符串，所以一般是字符串操作方法，使用`value`表示当前文本框的值。

```html
<input id="TextBox1" type="text" get="value.replace(/\s/g, '')" set="value.trim()" />
```

上例中，当获取文本框的值时，会执行`get`属性内的逻辑，自动将输入值去掉所有空白字符。当设置文本框的值时，会执行`set`属性的逻辑，自动去掉输入值两端的空白字符。

## INPUT 可用的选择器

可使用原生选择器获取或设置文本框的值，例如`$s('#TextBox1').value`。一般情况下用不到选择器。

## 其他扩展属性

除了可在在标签中定义的扩展属性之外，还有几个可用的扩展属性，这些属性不能在标签中定义。

* `required` 是否是必填项，注意已覆盖原生的`required`属性，这个根据已设置的扩展属性进行判断。
* `warnText` 获取或设置提醒文字。
* `status` 获取文本框的状态，分别有几个枚举值：`-1`输入值无效，`0`输入值为空，`1`输入值有效，`2`空值初始状态，`3`有值初始状态。文本框状态在其他组件中使用。

---
参考链接

* [root.js 基本库](/root.js/root.md)
* [Button 按钮扩展](/root.js/button.md)
* [Express 字符串](/root.js/express.md)
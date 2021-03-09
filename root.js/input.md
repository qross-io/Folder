# INPUT 标签扩展

为了更方便的处理表单中的用户输入，组件库对 INPUT 标签进行了扩展。

```html
<input type="text" name="Input1" required-text="" invalid-text="" warning-text="" valid-text="" validator="" minlength="0" class="" focus-class="" error-class="" valid-class="" error-text-class="" valid-text-class="" get="" set="" prevent-injection update-value-on="" />
```

扩展或新增的属性有：

* `type` 文本框的类型，除扩展了原生类型`text`、`number`和`password`外，还新增了`integer`、`float`、`mobile`、`idcard`和`name`。各类型的应用在正面分别介绍。
* `hint` 设置显示提醒文字的元素，值使用 CSS 选择器，如`#Message`。如果不设置这个属性，则默认在文本框的后面创建一个元素。使用`error-text-class`和`valid-text-class`设置文字样式。可以多个 INPUT 使用同一个元素标签，如只在提交按钮后面显示提醒。
* `callout` 如果设置了这个属性，则提醒文字在 Callout 中显示，这个属性可以设置 Callout 的显示位置，可选值有 `leftside`、`rightside`、`upside`、`downside`，默认值为`upside`。如果设置了`callout`属性而没有设置`hint`属性，则只用 Callout 的方式显示提醒；如果没有设置`callout`属性，则使用文本元素显示提醒文字；可以两种方式同时使用。
* `required-text` 当输入值为空时的提示文字，显示在输入框的右方。
* `invalid-text` 当输入值不满足验证要求时的提示文字，显示在输入框的右方。
* `warning-text` 当输入值满足验证要求但仍然不正确时要显示的提示文字，如用户名不唯一等。
* `valid-text` 当输入值正确时的提示文字，显示在输入框的右方，不设置则不显示。
* `validator` 正则表达式验证器，如`^[a-z]$`只允许输入小写英文字母，可以使用 Javascript 的格式，如`/^[a-z]$/i`来忽略大小写。
* `minlength` 最小字符长度，当不满足要求时提示`invalid-text`。小于等于`0`表示不限制最小长度。这个属性是原生属性。
* `autosize` 是否根据输入的字符数自动调整宽度`size`，中文占用两个字符位置，如果设置了这个属性，则`size`属性表示最小宽度。
* `class` 默认的显示样式，必填项默认值为`input-required-class`，选填项默认值为`input-optional-class`。
* `focus-class` 当文本框获得焦点时的样式，默认使用系统样式。
* `error-class` 当输入不正确时的样式，默认使用系统样式。
* `valid-class` 当输入正确时的样式，默认使用系统样式。
* `error-text-class` 警告文字的样式，默认为红色。
* `valid-text-class` 输入正确时的提醒文字的样式，默认为绿色，需设置`valid-text`才显示。
* `check` 接受一个接口地址或 PQL 查询语句（不一定是 SELECT），检查输入值是否符合预期，如检查用户名是否唯一。格式见 [data 属性](/root.js/data.md) 和 [Express 字符串](/root.js/express.md)。
* `get` 在获取输入值时对值进行加工，返回加工后的值，下面有详细的解释。
* `set` 在设置输入值时对值进行加工，设置输入值为加工后的值，下面有详细的解释。
* `prevent-injection` 自动在获取值时将单引号`'`转成两个单引号`''`以防止 SQL 注入，当使用这个文本框值的组件的动作为一条 SQL 时才有必要设置这个属性，接口的防 SQL 注入由后端控制。
* `update-{attr}-on` 指定文本框的值或其他属性的更新条件，其中`{attr}`可以替换成组件支持的属性，例如`update-value-on`，下面有详细的示例。属性值规则见[事件表达式](/root.js/event.md)。

特有 type 类型的属性有：

* `max` 原生属性，设置允许的最大值 ，适用于`number`、`integer`和`float`类型。
* `min` 原生属性，设置允许的最小值 ，适用于`number`、`integer`和`float`类型。
* `pad` 是否在小数点后不满足精度要求时自动补`0`，如精度为`4`时，值为`1.34`，则会自动修正为`1.3400`，适用于`number`和`float`类型。
* `positive` 是否为正，适用于`number`、`float`和`integer`类型，设置为`true`或其他可识别为`true`的值时，只允许输入正值。
* `precision` 小数点的精度，适用于`number`和`float`类型，为`0`时即控制为整数。
* `strength` 密码复杂度，可选值 `WEAK`、`STRONG`和`COMPLEX`，适用于`password`类型，用于密码强度控制。
* `fit` 设置一个密码输入框的 id，适用于第二个`password`，用于检查两次输入的密码是否一致。如`#Password`。

## 文本 type="text"

使用时需要引入：

```html
<script type="text/javascript" src="/root.js"></script>
<script type="text/javascript" src="/root.input.js"></script>
```

如果要使用 Callout 提示，还需要引入：

```html
<script type="text/javascript" src="/root.callout.js"></script>
```

<script type="text/javascript" src="@/root.input.js"></script>
<script type="text/javascript" src="@/root.callout.js"></script>

以下文本框只能输入英文字符：

 <input type="text" required-text="这个字段不能为空。" invalid-text="只能输入英文字符，且最少输入3个。" valid-text="输入正确。" validator="^[a-zA-Z]+$" minlength="3" autosize size="20" />

示例代码为：

 ```html
<input type="text" required-text="这个字段不能为空。" invalid-text="只能输入英文字符，且最少输入3个。" valid-text="输入正确。" validator="^[a-zA-Z]+$" minlength="3" autosize size="20" />
 ```

## 数字 type="number"

`number` 类型是 Html 的原生属性，只允许用户在输入框中输入数字，并且提供上下箭头以进行快速设置。root.js 对`number`类型的改变主要包括：

* 不再支持科学计数法，`e`和`+`不再允许输入。
* 只能在第一个字符输入`-`。
* 可以通过`positive`属性控制只能输入正数。
* 可以通过`precision`属性设置精度，当失去焦点时自动进行精度检查，四舍五入。当`precision`设置为`0`时，只能设置整数。
* 可以通过`pad`属性设置当精度不够时，自动在小数后补`0`，如精度为`4`时，`1.34`自动转化为`1.3400`，`2`自动转化为`2.0000`。
* 可以通过`min`和`max`属性对输入值进行限制，当输入值不满足要求时，会自动提示`invalid-text`。

```html
正整数：<input type="number" positive="yes" precision="0" min="0" max="100" step="10" invalid-text="必须输入 0 到 100 之间的值。" callout />
精度为4：<input type="number" pad="yes" precision="4" />
```

运行效果为：

正整数：<input type="number" positive="yes" precision="0" min="0" max="100" step="10" invalid-text="必须输入 0 到 100 之间的值。" style="width: 100px" callout /> -| 20 |-
精度为`4`：<input type="number" pad="yes" precision="4" style="width: 100px" />

`number`类型`maxlength`不可用，所有不能用来限制用户输入的长度，如果希望用`maxlength`来控制用户输入，请使用`integer`或`float`类型。原生属性`step`用来控制箭头按钮的步长。

## 整数 type="integer"

`integer` 是扩展类型，只允许在输入框中输入整数和`0`，不像`number`提供上下箭头用来快速设置。其他与`number`相同。

```html
<input type="integer" positive="false" min="-100" max="100" maxlength="3" invalid-text="必须输入 -99 到 100 之间的值。" />
```

运行效果为：

<input type="integer" positive="false" min="-100" max="100" maxlength="3" invalid-text="必须输入 -99 到 100 之间的值。" size="20" />

可以通过`maxlength`设置允许用户可输入的最大字符数。

## 小数 type="float"

`float` 也是扩展类型，除不提供上下箭头用来快速设置和支持`maxlength`外，其他均与`number`相同。

```html
<input type="float" positive="yes" pad="yes" precision="4" min="0" max="100" invalid-text="必须输入 0 到 100 之间的值。" />
```

运行效果为：

<input type="float" positive="yes" pad="yes" precision="4" min="0" max="100" invalid-text="必须输入 0 到 100 之间的值。" size="20" />

## 密码 type="password"

`password`是原生类型，root.js 对`password`的扩展主要是增加了`strength`属性，用来设置密码强度，可选值有`WEAK`，`STRONG`，`COMPLEX`。

* `WEAK` 密码位数不受限制，字符类型不受限制，默认值。
* `STRONG` 密码至少`6`位，必须包含英文字母和数字。
* `COMPLEX` 密码至少`8`位，必须包含大写、小写英文字母、数字和符号。

当输入密码强度不满足要求时，显示`invalid-text`提示。

```html
<input type="password" strength="strong" invalid-text="密码中必须包含英文字母和数字。" size="20" />
```

运行效果为：

<input type="password" strength="strong" invalid-text="密码中必须包含英文字母和数字，至少 6 位。" size="20" />

## 确认密码 fit="#Password"

这个属性用来检查两次输入密码的是否一致，属性值为第一次密码输入框的 id。

```html
请输入密码：<input type="password" id="Password" required-text="请输入密码。" /> <br/>
请再次输入密码：<input type="password" fit="#Password" invalid-text="两次输入的密码不一致。" /> 
```

运行效果为：

　　请输入密码：<input type="password" id="Password" required-text="请输入密码。" /> <br/>
请再次输入密码：<input type="password" fit="#Password" invalid-text="两次输入的密码不一致。" /> 

注意`fit`属性中的`#`不能省略，可理解为是一个 CSS 选择器。另外也使用`invalid-text`提示不匹配。

## 手机号 type="mobile"

`mobile`是扩展类型，允许输入 11 位手机号码，且第 1 位必须是`1`，第 2 位必须是`3`到`9`。文本框中只允许输入数字。

```html
<input type="mobile" size="20" value="1" />
```

运行效果为：

<input type="mobile" size="20" invalid-text="请输入正确的手机号码。" />

## 身份证号 type="idcard"

`idcard`也是扩展类型，允许输入18位身份证号码，除最后一位允许为`X`外，其他 17 位只能输入数字。

```html
<input type="idcard" size="30" invalid-text="请输入正确的身份证号码。" />
```

运行效果为：

<input type="idcard" size="30" invalid-text="请输入正确的身份证号码。" />

## 姓名 type="name"

`name`也是扩展类型，至少输入两个字符。

```html
<input type="name" size="20" invalid-text="姓名至少有两个字。" callout="rightside" />
```

运行效果为：

<input type="name" size="20" invalid-text="姓名至少有两个字。" callout="rightside" />

## 进一步对值进行检查

有时即使输入值符合要求，也要进一步对值进行检查，以判断是否符合系统要求，比如用户名唯一检查。

```html
<input type="name" minlength="6" maxlength="16" required-text="用户名不能为空。" invalid-text="用户名至少为 6 位，最多 16 位。" warning-text="用户名已经存在。" check="/system/check-username?username={value}" />
```

运行效果为：

<input type="name" minlength="6" maxlength="16" required-text="用户名不能为空。" invalid-text="用户名至少为 6 位，最多 16 位。" warning-text="用户名已经存在。" check="/api/system/check-username?username={value}" valid-text="用户名可以使用。" size="40" />

上例中，用户名`master`已经在系统中，`{value}`表示获取当前文本框的值。当文本框失去焦点时，会自动调用`check`属性中的接口，并判断返回值是否为空。当返回值不为空，即用户名已经存在时，会提示`warning-text`属性中的文字。

## 其他组件触发更新

通过标签的`update-{attr}-on`属性，可以设置一个[事件表达式](/root.js/event.md)，当表达式中的组件触发相应的事件时，更新文本框的值或其他属性。这个标签需要配合相应对应的值格式属性使用，如`value:`，注意属性名中多一个冒号`:`以和原有的属性`value`区分，`value:`属性值格式设置见 [Express 字符串](/root.js/express.md)。

```html
<select id="Select1">...</select>
<select id="Select2">...</select>
<input type="text" update-value-on="change:Select1" value:="$(#Select1).~{ this.value.takeAfter('.') }" update-size-on="change:#Select2" size:="$(#Select2)" />
```

上例中，当选择框`Select1`改变选项时，文本框的`value`自动更新，`value:`属性值中的`this`指向当前文本框。当选择框`Select2`选项改变时，文本框的`size`属性自动更新。

## 对输入值进行拦截

`get`和`set`属性可以实现拦截器的功能，当获取或设置`value`属性的值时，对值进行加工，并返回加工后的值。这两个属性的属性值是一个 Javascript 短句，因为输入值是一个字符串，所以一般是字符串操作方法，使用`value`表示当前文本框的值。

```html
<input id="TextBox1" type="text" get="value.replace(/\s/g, '')" set="value.trim()" />
```

上例中，当获取文本框的值时，会执行`get`属性内的逻辑，自动将输入值去掉所有空白字符。当设置文本框的值时，会执行`set`属性的逻辑，自动去掉输入值两端的空白字符。

## 可用的选择器

可使用原生选择器获取或设置文本框的值，例如`$s('#TextBox1').value`。

## 其他扩展属性

除了可在在标签中定义的扩展属性之外，还有几个可用的扩展属性，这些属性不能在标签中定义。

* `$required` 是否是必填项，区别于原生的`required`属性，这个会根据已设置的扩展属性进行判断。
* `hintText` 获取或设置提醒文字。
* `status` 获取文本框的状态，分别有几个枚举值：`-1`输入值无效，`0`输入值为空，`1`输入值有效，`2`空值初始状态，`3`有值初始状态，另外一个不常用值为`-2`表示值有效但是不符合预期。文本框状态在其他组件中使用。

---
参考链接

* [root.js 基本库](/root.js/root.md)
* [Button 按钮扩展](/root.js/button.md)
* [Express 字符串](/root.js/express.md)
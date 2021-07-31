# INPUT 标签扩展

为了更方便的处理表单中的用户输入，组件库对 [INPUT 标签](/root.js/input-native.md)进行了扩展。

```html
<input type="text" name="TextBox1" required-text="" invalid-text="" valid-text="" validator="" minlength="0" class="" focus-class="" error-class="" valid-class="" error-text-class="" valid-text-class="" get="" set="" set-value-on="" onchange+="server-event" success-text="" failure-text="" exception-text="" onload="" />
```

扩展或新增的属性有：

* `type` 文本框的类型，除扩展了原生类型`text`、`number`和`password`外，还新增了`integer`、`float`、`mobile`、`idcard`和`name`。各扩展类型的应用在下面分别介绍。
* `autosize` 是否根据输入的字符数自动调整宽度`size`，中文占用两个字符位置，如果设置了这个属性，则`size`属性表示最小宽度。
* `callout` 如果设置了这个属性，则提醒文字在 Callout 中显示，这个属性可以设置 Callout 的显示位置，可选值有 `leftside`、`rightside`、`upside`、`downside`，默认值为`upside`。如果设置了`callout`属性而没有设置`hint`属性，则只用 Callout 的方式显示提醒；如果没有设置`callout`属性，则使用文本元素显示提醒文字；可以两种方式同时使用。
* `class` 默认的显示样式，必填项默认值为`input-required-class`，选填项默认值为`input-optional-class`。
* `enter` 可以设定一个地址，当按下回车键时，自动跳转到指定的地址。如`<input type="search" enter="/search?key=$:[value]" />`。如果不设置值，那么当按下回车时，则失去焦点触发`onblur`或`onchange`事件。
* `error-class` 当输入不正确时的样式，默认使用系统样式。
* `error-text-class` 警告文字的样式，默认为红色。
* `exception-text` 服务器端事件`onchange+`执行失败时的提示文字，一般为接口出错。如果不设置则提示接口的出错信息。
* `failure-text`  服务器端事件`onchange+`执行成功并且结果验证未通过时的提示文字，如用户名重复等。
* `focus-class` 当文本框获得焦点时的样式，默认使用系统样式。
* `getter` 在获取输入值时对值进行加工，返回加工后的值，下面有详细的解释。
* `hint` 设置显示提醒文字的元素，值使用 CSS 选择器，如`#Message`。如果不设置这个属性，则默认在文本框的后面创建一个 SPAN 元素。使用`error-text-class`和`valid-text-class`设置文字样式。可以多个 INPUT 使用同一个元素标签，比如只想在提交按钮后面显示提醒。
* `if-empty` 设置一个值，当输入值为空时，自动赋为这个值。如`if-empty="60"`，当输入值为空时，那么自动填充值为`60`。
* `invalid-text` 当输入值不满足验证要求时的提示文字。
* `minlength` 最小字符长度，当不满足要求时提示`invalid-text`。小于等于`0`表示不限制最小长度。这个属性是原生属性。
* `required-text` 当输入值为空时的提示文字。
* `set-{attr}-on` 指定文本框的值或其他属性的更新条件，其中`{attr}`可以替换成组件支持的属性，例如`set-value-on`，下面有详细的示例。属性值规则见[事件表达式](/root.js/event.md)。
* `setter` 在设置输入值时对值进行加工，设置输入值为加工后的值，下面有详细的解释。
* `success-text` 服务器端事件`onchange+`执行成功并且结果验证通过时的提示文字，如检查用户名唯一等。
* `valid-class` 当输入正确时的样式，默认使用系统样式。
* `valid-text` 当输入值正确时的提示文字，不设置则不显示。
* `validator` 正则表达式验证器，区别于原生属性`pattern`，`pattern`是大小写敏感的，`validator`不区分大小写，如`pattern="^[a-z]$"`只允许输入小写英文字母，而`validator="^[a-z]$"`也可以输入大写字母。
* `valid-text-class` 输入正确时的提醒文字的样式，默认为绿色，需设置`valid-text`才显示。

扩展事件有：

* `onchange+` 服务器端事件，接受一个接口地址或 PQL 查询语句（不一定是 SELECT），检查输入值是否符合预期，如检查用户名是否唯一。详见[服务器端事件](/root.js/server.md)，属性格式见[与数据相关的属性](/root.js/data.md) 和 [Express 字符串](/root.js/express.md)。
* `onchange-checked` 当改变到选中状态时触发的事件，适用于`switch`和`checkbox`类型。
* `onchange-unchecked` 当改变到未选中状态时触发的事件，适用于`switch`和`checkbox`类型。
* `onload` 实现了原生未实现的事件，当文本框加载后触发。
* `onmodify` 新增的事件，当且仅当用户输入的值改变时触发。


以上以`-text`结尾的提醒文本属性均支持 [Express 表达式](/root.js/express.md)。

下面是一些特有 type 类型的属性：

* `max` 原生属性，设置允许的最大值 ，适用于`number`、`integer`和`float`类型。
* `min` 原生属性，设置允许的最小值 ，适用于`number`、`integer`和`float`类型。
* `pad` 是否在小数点后不满足精度要求时自动补`0`，如精度为`4`时，值为`1.34`，则会自动修正为`1.3400`，适用于`number`和`float`类型。
* `positive` 是否为正，适用于`number`、`float`和`integer`类型，设置为`true`或其他可识别为`true`的值时，只允许输入正值。
* `precision` 小数点的精度，适用于`number`和`float`类型，为`0`时即控制为整数。
* `strength` 密码复杂度，可选值 `WEAK`、`STRONG`和`COMPLEX`，适用于`password`类型，用于密码强度控制。
* `fit` 设置一个密码输入框的 id，适用于第二个`password`，用于检查两次输入的密码是否一致。如`#Password`。
* `options` 仅适用于`checkbox`和`switch`类型，指定复选框或切换按钮在两种状态下的值，例如`yes,no`、`1,0`等
* `theme` 仅适用于`switch`类型，指定切换按钮的主题，默认值`switch`，可选值`checkbox`和`whether`。



其他原生属性请参照 [INPUT 标签](/root.js/input-native.md)。

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

## 复选框

`checkbox`是原生类型，可使用`options`来设定选中和未选中状态下的值，也可以使用`onchange+`事件在状态切换时与服务器端交互。

```html
<input type="checkbox" options="yes,no" required="true" onchange+="put:/api/entity/enabled?id=&(id)&status={value}" onchange-checked-="show: #Agreement" />
```

## 切换按钮

`switch`是扩展类型，这个类型表现为一个切换按钮，按钮有两种状态，开启状态和关闭状态，点击时在两种状态之是切换。可以理解为是复选框的图片版本。可使用`theme`设置按钮的样式。

```html
<input type="switch" options="1,0" onchange+="put:/api/licence/open?id=&(id)&status={value}" onchange-unchecked-="hide: #Message" />
```

## 进一步对值进行检查

有时即使输入值符合要求，也要进一步对值进行检查，以判断是否符合系统要求，比如用户名唯一检查。使用`onchange`事件（当值改变时且失去焦点）触发。

```html
<input type="name" minlength="6" maxlength="16" required-text="用户名不能为空。" invalid-text="用户名至少为 6 位，最多 16 位。" onchange+="/system/check-username?username={value} -> empty" success-text="用户名可以使用。" failure-text="用户名已经存在。" />
```

运行效果为：

<input type="name" minlength="6" maxlength="16" required-text="用户名不能为空。" invalid-text="用户名至少为 6 位，最多 16 位。" onchange+="/api/system/check-username?username={value} -> empty" success-text="用户名可以使用。" failure-text="用户名已经存在。" size="40" />

上例中，用户名`master`已经在系统中，`{value}`表示获取当前文本框的值。当文本框失去焦点时，会自动执行`onchange+`事件，并判断返回值是否为空。当返回值不为空，即用户名已经存在时，会提示`failure-text`属性中的文字，否则提示`success-text`中的文字。

## 其他组件触发更新

通过标签的`set-{attr}-on`属性，可以设置一个[事件表达式](/root.js/event.md)，当表达式中的组件触发相应的事件时，更新文本框的值或其他属性。值格式设置见 [Express 字符串](/root.js/express.md)。

```html
<select id="Select1">...</select>
<select id="Select2" onchange+="..">...</select>
<input type="text" set-value-on="load; change:Select1 -> $(#Select1).~{ this.value.takeAfter('.') }" set-size-on="change+success:#Select2 -> $(#Select2)" />
```

上例中，当文本框加载后和选择框`Select1`改变选项时，文本框的`value`自动更新，上式中的`this`指向当前文本框。当选择框`Select2`选项改变时，文本框的`size`属性自动更新。

## 对输入值进行拦截

`getter`和`setter`属性可以实现拦截器的功能，当获取或设置`value`属性的值时，对值进行加工，并返回加工后的值。这两个属性的属性值是一个 Javascript 短句，因为输入值是一个字符串，所以一般是字符串操作方法，使用`value`表示当前文本框的值。

```html
<input id="TextBox1" type="text" getter="value.replace(/\s/g, '')" setter="value.trim()" />
```

上例中，当获取文本框的值时，会执行`getter`属性内的逻辑，自动将输入值去掉所有空白字符。当设置文本框的值时，会执行`setter`属性的逻辑，自动去掉输入值两端的空白字符。

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
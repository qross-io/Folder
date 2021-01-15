# root.js 基础库

**root.js** 是整个组件库的基础，root.js 基础库提供类似 **jQuery** 的功能，但是对细节更容易控制。组件库的其他组件都是在 root.js 基础上进行开发。

## String 类型扩展方法

* `$trim(char1, char2)` 移除字符串两端的指定字符，如移除左右括号 `str.$trim('(', ')')`
* `$includes(str, delimiter = ',')`  判断字符串是否包含指定字符，类似于 SQL 中的`in`，如字符串`'1,3,14,15,28'`中是否包含`'4'`，返回`false`，是否包含`14`，返回`true`
* `$remove(str, delimiter = ',')`  如字符串 `'1,3,14,15,28'` 中移除`14`，返回`'1,3,15,28'`
* `$length(min = 0)` 可以把中文字符识别为`2`个位置，`'中文'.$length()`结果为`4`

 几个字符串转化方法：

* `toInt(defaultValue = 0)`  将字符串转化为整数，比`parseInt`方法更智能
* `toFloat(defaultValue = 0)`  将字符串转化为小数，比`parseFloat`方法更智能
* `toBoolean(defaultValue = false)` 将字符串转化为布尔值，会把`yes`, `1`, `true`, `ok`, `on`识别为`true`，会把`no`, `0`, `false`, `cancel`, `off`识别为`false`
* `toArray(delimiter = ',')` 将字符串转化转化数组，类似于`split`方法，也可以把`'[1,2,3,4]'`解析成数组
* `toMap(delimiter = '&', separator = '=')` 将字会串转化为 Object 结构，比较常用的是地址参数
* `toJson()` 将字符串转成Json对象

其他可用的转化方法：

* `toCamel()` 将分隔符形式的字符串转驼峰格式，如`'mine-type'.toCamel()`结果是`mineType`
* `toHyphen()` 将驼峰格式的字符串转分隔符格式，如`'mineType'.toHyphen()`结果是`mine-type`


## Number 类型扩展方法

* `kilo()` 为数字增加千分符，返回字符串。如`12004.kilo()`结果是`'12,004'`
* `toPercent(digits = 2)` 将数字转换为百分数字符串，默认保留两位小数。如`0.65487.toPercent()`结果是`65.49%`

## 全局方法

全局转化方法，可处理`null`值，如果传入值为`null`，则返回默认值

* `$parseString(value, defaultValue = '')` 将数值转成字符串
* `$parseInt(value, defaultValue = 0)`  将数值转成整数
* `$parseFloat(value, defaultValue = 0)` 将数值转成小数
* `$parseBoolean(value, defaultValue = false)` 将数值转成布尔值
* `$parseArray(value, defaultValue = [])` 将数值转成数组

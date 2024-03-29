# 开发自己的组件


## Javascript 命名规范

* 全局函数变量，以`$`开始，如`$root`
* 变量名、属性名、方法名，Camel表示法，第二个单词开始首字母大写，如`helloWorld`
* 类名，Pascal 表示法，所有单词首字母大写，如`SelectButton`、`TreeView`
* 私有变量，以`_`开头，如`_value`
* 枚举值，字母全部大小，多个单词之间以`_`分隔，如`Gender.MALE`或`Display.SHOW_CHECKBOX`

## HTML 和 CSS 命名规范

* 标签名称小写，尽量不使用多单词标签
* 属性名，全部字母小写，多个单词之间使用`-`之间隔开，如`open-mode="text"`
* /s:隐藏变量，以`-`开头，如`-value`/
* CSS 名称，全部字母小写，多个单词之间使用`-`之间隔开，如`.select-focus`
* 标签或元素 id，Pascal 表示法，所有单词首字母大写，如`ConnectionName`

## URL 地址

* 路径、文件名及文件扩展名全部小写
* 参数名称全部小写，单词之间使用下划线隔开。也可使用 camel 或 Pacal 表示法。
* 锚点名称依照元素 id 或名称确定。

## Javascript 中空格的使用

* `if`、`for`、`function`后面使用空格
* `:`后面使用空格
* 操作符两边加空格，如`let a = 1;`

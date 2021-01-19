# Voyager 支持 Markdown 文件

除了 Html 格式外，Voyager 还支持 Markdown 格式的文档，扩展名使用`.md`。

在使用 Markdown 作为页面文件格式时，强烈建议使用[母版页](/voyager/master.md)，以更好的控制整个页面的布局。

Voyager 使用 [Marker 类](/voyager/marker.md)将 Markdown 格式转成 HTML 格式，[Marker 类](/voyager/marker.md)也可以你自己的项目中使用。

Marker 不仅可以对标准的 Markdown 文档格式进行转换，还提供了一些扩展，可以让 Markdown 提供更多的样式和功能。

## Marker 对 Markdown 的扩展

### 文字样式

Markdown 本身没有提供对文字颜色、大小和斜体等样式的控制，Marker 扩展了 Markdonw 的语法，以提供文字样式控制功能。

```
/red:红色字体/
/blue,24:蓝色大字/
/32,i,b:粗体和斜体大字/
/green:绿色字体橙色背景:orange/
```

如上例所示，语法两个斜杠、样式部分、起分隔作用的冒号`:`、分隔样式的逗号`,`和文本内容组成。可控制的样式有：

* 字体大小，是一个大于0的整数数字，`16`为默认大小。
* 字体颜色单词，如`green`、`yellow`等，支持的颜色及对应的编码见下表。
* 字体颜色编码，如红色对应`#FF0000`，详见[颜色编码表](/root.js/colors.md)。
* 字体样式字母，支持4个：`b`表示粗体，`i`表示斜体，`u`表示下划线，`s`表示删除线，其中下划线和删除线不能共存。
* 文字背景色，写在文字内容的后面，如`/黄色背景:yellow/`。


---
参考链接

* [Voyager 模板引擎](/voyager/overview.md)
* [Marker 类](/voyager/marker.md)
* [颜色编码表](/root.js/colors.md)
# Voyager 母版页

**母版页** 就是预先定义一个模板，这个模板包含框架、布局、样式等通用信息，其他页面开发时可以引用这个模板，在这个模板基础上进行开发，以避免复制粘贴或重复开发。

## 母版页定义

母版页必须是`.html`文件，可以放在站点的任何目录下，比如`/template/form.html`。示例代码如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>#{title}</title>
    <script type="text/javascript" src="/root/root.js"></script>
    <link href="/root/root.css" rel="stylesheet" type="text/css" />
    <link href="/root/iconfont.css" rel="stylesheet" type="text/css" />
    <#include file="/bar.html"/>
    #{scripts}
</head>
<body>
<#include language="#{language}"/>
<table border="0" width="100%" cellspacing="0" cellpadding="6" class="title-area" fixed="yes">
    <tr>
        <td class="crumbs"><a class="primary-title" href="#{back}">#{previous}</a> <i class="iconfont icon-right"></i> #{crumb}</td>
        <td align="center">&nbsp;</td>
        <td class="jump-button">
            &lt;% IF $button IS UNDEFINED THEN %&gt;
                &nbsp;
            &lt;% ELSE %&gt;
                <button href="#{jump}" class="normal-button #{button_class}"> &nbsp; &nbsp; #{button} &nbsp; &nbsp; </button>
            &lt;% END IF %&gt;
        </td>
    </tr>
</table>
<div class="space"></div>
<div class="full-frame">
    #{content}
</div>
</body>
</html>
```

在母版中，支持所有 Voyager 特性，比如参数、服务器端包含、引入语言包、服务器端逻辑等，就和一个不使用母版的页面一样。

* 参数，上例中`#{title}`、`#{scripts}`都是参数，如果在引用母版时传递参数在下一节介绍。
* 服务器端包含，如上例中`<#include file="/bar.html"/>`
* 语言包引入，如上例中`<#include language="#{language}"/>`，这里使用了参数`#{language}`
* 服务器端逻辑，如上例中`<~u0025 IF $button IS UNDEFINED THEN ~u0025>`

在母版页中，有几个保留参数：

* `#{title}` 可以向母版页传递`title`参数，如果不传则自动解析 Markdown 文档中的主标题
* `#{scripts}` Voyager 根据引用的组件自动判断引入哪些 Javascript 文件和 CSS 样式文件，不可传参
* `#{content}` 使用母版页的页面在被解析后替换这个位置，是子页面的内容页代码，不可传参

## 母版页引用

在子页面中需要指定页面使了哪个母板页，子页面解析后（如从 Markdonw 转成 HTML）会自动替换母板页`#{content}`所在的位置。

通过以下语法指定母版面:

```html
<# page template="/master/form.html" />
```

地址相对于整个站点，可以放在站点的任何位置。

多数情况下，我们需要向母版页传递参数，以上母版页显示不同的内容。

```html
<# page template="/master/form.html?language=system&jump=/link/detail%3Fname%3DTome%26age%3D18&button=Create%20Item" />
```

上例中，与地址参数冲突的字符需要用特殊占位符代替，对应表如下：

* 问号`?`，使用`%3F`代替
* 行号`=`，使用`%3D`代替
* 与号`&`，使用`%26`代替
* 空格` `，使用`%20`代替

上例中，参数`name`会替换母版页中所有的`#{name}`，其他参数同理。


---
参考链接

* [Voyager 模板引擎](/voyager/overview.md)
* [Voyager 语法](/voyager/syntax.md)
* [Marker 类](/voyager/marker.md)
* [Voyager 中的 Markdown 文档格式](/voyager/markdown.md)

# Voyager 国际化支持

Voyager 支持根据不同的设置或用户习惯在页面上显示不同的语言，配置非常简单。

## 在 resources 文件夹下添加语言文件夹和文件

语言配置文件放在 resources 文件下的`languages`文件夹下，没有自行创建即可。然后在`languages`文件夹下创建两个语言文件夹，如`chinese`和`english`。语言配置文件保存在相应的语言文件夹下，扩展名为`.lang`，一般一个模块创建一个语言文件即可。可以创建多层文件夹，比如父级模块、子模块等，一般对于中小型项目，一层目录足够用了。创建好的文件目录结构如下：

+ resources
    + languages
        + chinese
            + carton.lang
            + module1.lang
            + module2.lang
        + english
            + carton.lang
            + module1.lang
            + module2.lang
    + templates

语言文件的名称可根据模块的名称设置，建议是英文单词，多个单词之间使用中线`-`隔开，如`job-task`。不同语言文件夹下的语言文件名必须一一对应，否则切换语言时找不到相应的配置。

## 在语言文件中添加语言标签

语言文件使用 properties 文件相同的设置规则，即`标签名=语言文字`。

在 chinese 文件夹下的`carton.lang`文件中输入

```properties
tom=汤姆
jerry=杰瑞
```

在 english 文件夹下的`carton.lang`文件中输入

```properties
tom=Tom
jerry=Jerry
```

标签名可以由英文字母、数字和中线`-`组成，多个单词之间可以由`-`分隔，一定不能包含等号`=`和小数点`.`。等号左侧标签名不能有空格，但是右侧语言文字可以有空格。

## 在页面上引用语言标签

语言标签及对应的语言文字设置好之后，可以在页面上引用，格式为`# 语言标签 #`，中间的空格可有可无。示例如下：

```html
<body>
<h1 title="# carton.jerry #"># carton.tom #</h1>
</body>
```

如果当前语言是中文，则执行结果为

```html
<body>
<h1 title="杰瑞">汤姆</h1>
</body>
```

语言标签可以使用在页面的任何地方，包括 PQL 语句中的字符串常量。例如

```html
<div><%="# carton.tom # and # carton.jerry #"%></div>
```

当一个页面要使用非常多语言标签时，如果每个占位符都输入模块名会非常麻烦，可以通过`include`语法引入整个模块，引入之后可省略模块名。

```html
<body>
<#include language="carton"/>
<h1 title="# jerry #"># tom #</h1>
</body>
```

支持同时引入多个语言模块，模块名之间使用逗号`,`隔开，但如果不同的模块之间有相同名称的语言标签，会使用先引用的模块中的语言标签。未引入语言模块的标签依然可以通过原来的点规则引用。

## 如何切换语言

可以在配置文件中设置默认语言，详见 [Voyager 设置](/voyager/setup.md)中的`voyager.language`项。

一般情况下需要根据用户的操作系统语言或用户自行更改界面语言，Voyager 可以将用户设置保存在 Cookie 中，并会优先调取 Cookie 中的值，Cookie 名称为 `language`。即要切换语言，更新`language`的值即可。

---
参考链接

* [Voyager 设置](/voyager/setup.md)

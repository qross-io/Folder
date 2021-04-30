# 接收和处理 URL 地址中的参数

通过地址传参是最常用的传递数据的方式，在 Voyager 中处理参数非常简单。

例如地址`/carton/page?id=1&name=Tom&age=18`。

一种方法是通过PQL参数占位符 `#{name}`，可以在页面的任何地方使用，包括服务器端代码，比如：

```html
<div>姓名：#{name}</div>
<div>年龄：#{age}</div>
<div><%=SELECT * FROM scores WHERE id=#{id} -> TO HTML TABLE %></div>
```

另一种方法是通过 PQL 变量，Voyager 会把所有的地址参数转成变量，例如上面地址会生成`3`个变量：

```html
<div>姓名：<%=$name%></div>
<div>年龄：<%=$age%></div>
<div><%=SELECT * FROM scores WHERE id=$id -> TO HTML TABLE %></div>
```

注意所有参数变量的数据类型都为文本字符串。

## 母版页中的传递参数

```html
<# page template="/master/form.html?language=system&jump=%2Flink%2Fdetail%3Fname%3DTome%26age%3D18&button=Create%20Item" />
```

母版页地址中可以传递参数目的是为了定制出不同的母版页，字符冲突时需要将特殊字符进行编码，见本文后面的说明

## 服务器端包含中传递参数

同样，服务器端包含文件也可以带参数，目的也是为了定制引入页或页面上的内容。

```html
<#include file="top.html?title=学生成绩&link=/page/score%3Fid%3D#{id}">
```

参数值中与地址参数常用符号冲突时需要进行编码，见本文后面的说明。`#{id}`是页面地址中的参数。

## 参数中的特殊字符

母版页和服务器端中与地址参数冲突的字符需要进行 URL 编码，对应表如下：

* 问号`?`，使用`%3F`代替
* 行号`=`，使用`%3D`代替
* 与号`&`，使用`%26`代替
* 空格` `，使用`%20`代替
* 斜杠`/`，使用`%2F`代替，大多数情况下不必要
* 百分号`%`，使用`%25`代替

## 参数处理逻辑

在 Voyager，可以通过以上三种形式接收地址中的参数。这些参数可以在页面、母版页、服务器端包含文件 **任何地方** 以本文开头提到的两种方式使用，即每个参数的作用域是全局的，而不是针对于母版页或某一个服务器端包含文件，所以这些参数不建议使用相同的名字。如果使用了相同的名字，比如页面地址有`name`参数，母版页中也有`name`参数，服务器端包含文件中也有`name`参数，那么最后`name`的值是服务端包含文件参数中设定的值，即服务器端包含文件优先级最高，其次是母版页，最后是页面地址参数。

特别说明一点，页面地址中的参数可以传递给母版页和服务器端包含文件，例如有地址`/list?id=2`，页面中有：

```html
<# page temlate="/view.html?jump=%2Fdetail%2F#{id}" />
<# include file="/head.html?jump=%2Fcategory%3Fid=#{id}" />
```

上例中，`#{id}`部分会被替换为`2`。

---
参考链接

* [通过 model 向页面传递数据](/voyager/model.md)
# Voyager 全局设置

Voyager 的设置保存在项目`resources`目录下的`conf.properties`文件中。各个设置及简要说明如下：

```properties
# 在项目中默认保存模板文件的目录
voyager.directory=/templates/

# 静态站点路径，说明见下
voyager.static.site=

# 图库站点路径，说明见下
voyager.gallery.site=

# 默认的编码格式
voyager.charset=UTF-8

# 默认的数据源连接名
voyager.connection=mysql.qross

# 默认语言
voyager.language=english
```

以上设置中，默认数据库连接名如果不设置（留空）则使用`jdbc.default`，数据连接设置详见[数据源配置](/pql/properties.md)。默认语言的名称必须与`languages`下的语言文件夹名称一致，区分大小写。

上面设置中提到了静态站点路径和图片站点路径，系统允许将静态文件如 javascript 文件或 CSS 文件、图片文件放在其他网站中，并且可以统一进行设置。例如

```properties
voyager.static.site=http://s.qross.cn
voyager.gallery.site=http://i.qross.cn
```

设置后可以这样调用：

```html
<script type="text/javascript" src="@/root.treeview.js"></script>
<link href="@/css/root/main.css" rel="stylesheet" type="text/css" />
<img src="%/qross/logo.png" />
```

如上例所示，在路径前增加`@`表示使用静态站点路径，在路径前`%`表示使用图库站点路径，程序在服务器端进行解析。

---
参考链接

* [Voyager 多语言设置](/voyager/language.md)
* [PQL 数据源配置](/pql/properties.md)
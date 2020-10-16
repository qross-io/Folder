# Voyager 全局设置

Voyager 的设置保存在项目`resources`目录下的`conf.properties`文件中。各个设置及简要说明如下：

```properties
# 在项目中默认保存模板文件的目录
voyager.directory=/templates/

# 模板文件扩展名
voyager.extension=html

# 默认的编码格式
voyager.charset=UTF-8

# 默认的数据源连接名
voyager.connection=mysql.qross

# 默认语言
voyager.language=english
```

以上设置中，默认数据库连接名如果不设置则使用`jdbc.default`，数据连接设置详见[数据源配置](/pql/properties.md)。默认语言的名称必须与`languages`下的语言文件夹名称一致，区分大小写。

---
参考链接

* [Voyager多语言设置](/voyager/language.md)
* [PQL数据源配置](/pql/properties.md)
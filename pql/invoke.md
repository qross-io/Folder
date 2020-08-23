# 运行Java静态方法 INVOKE

通过INVOKE语句，可以在PQL中调用Java中的静态方法。示例如下：
```sql
SET $key := "VERSION";
SET $value := "0.6.4";
INVOKE io.qross.setting.Configurations.set($key, Object $value);
```

* 必须书写完整的Java类的路径，如上例`io.qross.setting.Configurations`
* 方法必须是静态方法，上例的`set`的定义为`public static void set(String key, Object value);`
* 方法中的参数一般会自动识别，识别不正确时请直接指定Java中的类型名，基本类型可以使用单词表示。比如整数的类型名为`int`或`integer`，不区分大小写。非基本类型需要使用Java类的全路径。
* INVOKE语句不返回任何值，仅调用。

Java实例方法或其他功能会在将来版本中支持。 

---
参考链接
* [在PQL中运行Shell命令](/pql/run.md)
* [PQL基本语法](/pql/basic.md)
* [PQL中的Javascript](/pql/javascript.md)
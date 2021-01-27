# 与Java交互 INVOKE语句

通过INVOKE语句，可以在PQL中调用Java中的静态方法和属性。示例如下：
```sql
SET $key := "VERSION";
SET $value := "1.1.0";
INVOKE io.qross.setting.Configurations.set($key, Object $value);

SET $version := INVOKE io.qross.setting.Global.VERSION;
```

* 必须书写完整的Java类的路径，如上例`io.qross.setting.Configurations`
* 方法必须是静态方法，上例的`set`的定义为`public static void set(String key, Object value);`
* 方法中的参数一般会自动识别，识别不正确时请直接指定Java中的类型名，基本类型可以使用单词表示。比如整数的类型名为`int`或`integer`，不区分大小写。非基本类型需要使用Java类的全路径。
* 对于不返回值的静态方法`public static void`，INVOKE语句返回`null`。
* 对Java类的静态方法和属性支持较好，但是对Scala类中带基本类型参数的暂时不好用。

和其他[有返回值的语句](/pql/evaluate.md)一样，可以将静态方法和属性的返回值赋值给变量，或者通过Sharp表达式对结果再编辑，或者作为查询表达式嵌入到其他语句中。Java实例方法会在将来版本中支持。 

---
参考链接

* [PQL中有返回值的语句](/pql/evaluate.md)
* [在PQL中运行Shell命令](/pql/run.md)
* [PQL基本语法](/pql/basic.md)
* [PQL中的Javascript](/pql/javascript.md)
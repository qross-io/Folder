# 与 Java 交互 INVOKE 语句

通过 INVOKE 语句，可以在 PQL 中调用 Java 中的静态方法和属性。示例如下：

```sql
SET $key := "VERSION";
SET $value := "1.1.0";
INVOKE io.qross.setting.Configurations.set($key, Object $value);

SET $version := INVOKE io.qross.setting.Global.VERSION;
```

* 必须书写完整的 Java 类的路径，如上例`io.qross.setting.Configurations`
* 方法必须是静态方法，上例的`set`的定义为`public static void set(String key, Object value);`
* 方法中的参数一般会自动识别，识别不正确时请直接指定Java中的类型名，基本类型可以使用单词表示。比如整数的类型名为`int`或`integer`，不区分大小写。非基本类型需要使用 Java 类的全路径。
* 对于不返回值的静态方法`public static void`，INVOKE 语句返回`null`。
* 对 Java 类的静态方法和属性支持较好，在 Scala 类`class`和单例对象`object`中如果声明了同名方法，单例对象中的方法会调取不到。

和其他[有返回值的语句](/pql/evaluate.md)一样，可以将静态方法和属性的返回值赋值给变量，或者通过 Sharp 表达式对结果再编辑，或者作为查询表达式嵌入到其他语句中。Java 实例方法会在将来版本中支持。 

---
参考链接

* [PQ L中有返回值的语句](/pql/evaluate.md)
* [在 PQL 中运行Shell命令](/pql/run.md)
* [PQL 基本语法](/pql/basic.md)
* [PQL 中的 Javascript](/pql/javascript.md)
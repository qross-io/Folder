# INVOKE 语句 - 与 Java 交互

通过 INVOKE 语句，可以在 PQL 中调用 Java 中的静态方法和属性。示例如下：

```sql
SET $key := "VERSION";
SET $value := "1.1.0";
INVOKE io.qross.setting.Configurations.set($key, Object $value);

SET $version := INVOKE io.qross.setting.Global.VERSION;
```

* 必须书写完整的 Java 类的路径，如上例`io.qross.setting.Configurations`
* 方法必须是静态方法，上例的`set`的定义为`public static void set(String key, Object value);`
* 方法中的参数一般会自动识别，识别不正确时请直接指定 Java 中的类型名，基本类型可以使用单词表示。比如整数的类型名为`int`或`Integer`，注意区分大小写，因为在 Java 中，`int`和`Integer`不是同一个类，一般使用前者。此规则同样适用于`long`和`Long`、`float`和`Float`、`double`和`Double`、`char`和`Character`。如果不指定类型，会优先识别为前者。非基本类型需要使用 Java 类的全路径。当不指定类型时，整数会自动识别为`int`，如果要使用长整型`long`，则需要在数字后面加字母`l`，不区分大小写，如`100L`。同理小数会自动识别为单精度浮点数`float`，如果要双精度数`double`，则值后面需要加`d`，不区分大小写，例如`1.23d`。当然单精度数后面也可以加`f`，如`120F`，以让程序自动识别为`float`。
* 对于不返回值的静态方法`public static void`，INVOKE 语句返回`null`。
* 对 Java 类的静态方法和属性支持较好，在 Scala 类`class`和单例对象`object`中如果声明了同名方法，单例对象中的方法会调取不到。

和其他[有返回值的语句](/pql/evaluate.md)一样，可以将静态方法和属性的返回值赋值给变量，或者通过 Sharp 表达式对结果再编辑，或者作为查询表达式嵌入到其他语句中。Java 实例方法可能会在将来版本中支持。 

---
参考链接

* [PQ L中有返回值的语句](/pql/evaluate.md)
* [在 PQL 中运行Shell命令](/pql/run.md)
* [PQL 基本语法](/pql/basic.md)
* [PQL 中的 Javascript](/pql/javascript.md)
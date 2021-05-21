# PQL 中对参数的处理

PQL 可以理解为一个计算过程，这个过程类型一个函数体。所以，PQL 过程支持传入并接收参数，根据传入的参数不同，计算过程和结果也会不同。本文主要说明一下 PQL 中接收和处理参数的逻辑，关于传递参数相关的介绍查看各相应的组件。

* [在 OneApi 中传递参数给接口](/oneapi/params.md)
* [在 Voyager 中传递参数给页面](/voyager/query.md)
* [在 Keeper 中传递参数给工作流的命令](/keeper/dag.md)

假如以下 PQL 逻辑：

```sql
UPDATE students SET title=$title, description='#{'description'}', release_date='#{date}' WHERE id=#{id};
```

上例中传递了四个参数：`title`、`description`、`date`和`id`，书写方法涵养了参数的所有占位符形态。

* `#{date}`和`#{id}` 参数的标准接收方式，PQL 在解析前就会把这个占位符替换为相应的参数值，不检查数据类型。因为 date 参数对应的 release_date 字段是日期时间类型，所以两端要加引号。
* `#{'description'}` 与上一种方式基本相同，也不判断数据类型，在参数名两端加单引号是为了防止注入攻击或者是为了将参数值中的单引号进行替换。同理，双引号的处理是`"#{"description"}"`
* `$title` PQL 会把所有的参数都自动转成变量，在 Json 方式传递参数时`{ "name": "Tom", "age": 18}`变量类型会自动判断，这时`age`的类型是整数。但在使用查询字符串`name=Tom&age=18`传递参数时无法判断，统一识别为字符串，这时`age`是的值`18`是字符串。上面的三个参数，也有其对应的变量`$data`、`$id`和`$description`，可视情况使用。参数变量在被嵌入到语句中时自动根据类型决定是否添加引号。注意如果把上例中的`description='#{'description'}'`改成`description=$description`是不建议的，因为变量不会自动处理有可能产生冲突的单引号。

在 PQL 过程中判断是否传递了参数，使用`UNDEFINED`关键字，以下两种方式都可以。

```sql
IF $title IS UNDEFINED THEN
    SET $title := 'Hello World';
END IF;
-- 或者
IF '#{title}' IS UNDEFINED THEN
    SET $title := 'Hello World';
END IF;
```

上例的作用是假如参数`title`未传递，那么就给`title`赋一个默认值`Hello World`。PQL 会自动检查过程中的每一个参数，并把这些参数声明为作用于整个 PQL 过程的变量，所以上例中`$title`的作用域是整个 PQL 过程，而不是 IF 语句块内。

---
参考链接

* [在 OneApi 中传递参数给接口](/oneapi/params.md)
* [在 Voyager 中传递参数给页面](/voyager/query.md)
* [在 Keeper 中传递参数给工作流的命令](/keeper/dag.md)
* [通过 Spring Boot 中 Controller 的 model 向页面传递数据](/voyager/model.md)
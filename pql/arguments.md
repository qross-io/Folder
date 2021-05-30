# 向 PQL 过程传递参数

任何程序都可以接受外部参数以确定执行不同的操作，PQL 过程一样也可以接受外部传递的参数。PQL 可以理解为一个计算过程，这个过程类型一个函数体。所以，PQL 过程支持传入并接收参数，根据传入的参数不同，计算过程和结果也会不同。

## 向 PQL 过程传递参数

任何与 PQL 相关的组件都支持传递参数，下面分别说明。

* 在 [Keeper 调度工具]中执行 PQL 命令时：
  工作流中任何类型的命令都支持传递参数，更多信息请参照[在 Keeper 中传递参数给工作流的命令](/keeper/dag.md)。

* 手工执行 PQL 文件时
  命令可以在本地开发环境和服务器上运行。
  ```sh
  java -jar qross-worker-1.5.0.jar --file /usr/qross/pql/test.sql --args name=Tom&age=18&score=89
  ```

* OneApi 传递参数
  OneApi 使用 Http 机制传递参数，比如 URL 地址或 Json 参数，详细请参阅[在 OneApi 中传递参数给接口](/oneapi/params.md)
  ```
  http://yourdomain.com/api/test?name=Tom&age=18&score=89
  ```

* Voyager 模板引擎中传递参数
  模板引擎也使用 Http 传递参数，例如`http://yourdoman.com/page/test?name=Tom&age=18&score=89`。Voyager 不只可以接收浏览器地址栏中输入的参数，也支持在母版页和包含文件中传递参数，更多信息请参阅[在 Voyager 中传递参数给页面](/voyager/query.md)

* 使用 PQL 类运行 PQL 时
  PQL 类提供了传递参数的多个重载方法，见 [PQL 类](/pql/class.md)。

## 使用占位符接收参数

第一种方式是在 PQL 设置占位符，如`#{name}`，见下例：

```sql
UPDATE students SET score=#{score} WHERE name='#{name}';
```

`#{`和`}`包围的内容仅仅是一个占位，会把传递过来的数据原封不动的替换占位符。上例在替换占位符之后就变成了：

```sql
UPDATE students SET score=89 WHERE name='Tom';
```

特别的，因为不对数据做处理，也可能出现引号冲突或 SQL 注入问题，比如上面`name`参数中带有单引号，会让 UPDATE 语句出现执行错误。PQL 已经提供了解决方法：

```sql
UPDATE students SET description='#{'description'}' WHERE id=#{id};
```

就是在参数名两端再加一对单引号`#{'description'}`，对于双此号场景，同样也支持。

```sql
UPDATE students SET description="#{"description"}" WHERE id=#{id};
```

如果包围参数占位符的是单引号，处理冲突就用单引号; 如果包围占位符的是双引号，就用双引号处理。

因为参数占位符不处理数据类型，并且在 PQL 其 **执行优先级最高**，所以我们可以用参数占位符干很多事。

```sql
SELECT * FROM movie_#{age} WHERE movie_name='#{name} and Jerry'; 
```

替换成参数值就变成了
```sql
SELECT * FROM movie_18 WHERE movie_name='Tom and Jerry';
```

参数占位符必须由`#{`和`}`包围，大括号不可省略。占位符可以设置在 PQL 任意的位置，甚至是语句关键词。需要注意的是，未传递变量时参数占位符不会被替换，仍保持原状态`#{name}`，很可能会导致程序运行出错或结果不正确，请一定认真检查。

## 使用参数变量

第二种方式是使用参数变量，PQL 会把所有的参数都自动转成变量，如`$name`：

```sql
UPDATE students SET score=$score! WHERE name=$name;
```

参数变量的数据类型由传递方式决定，在 Json 方式传递参数时`{ "name": "Tom", "age": 18}`变量类型会自动判断，这时`age`的类型是整数。但在使用查询字符串`name=Tom&age=18`传递参数时无法判断，统一识别为字符串，这时`age`是的值`18`是字符串。上面的三个参数，也有其对应的变量`$data`、`$id`和`$description`，可视情况使用。参数变量在被嵌入到语句中时自动根据类型决定是否添加引号。

当这些变量类型都是字符串类型，如果是数字使用时需要自已转换。如上例`$score!`，叹号`!`表示忽略引号。 用户变量不需要考虑引号冲突和 SQL 注入的问题，可以根据自己的喜好选用哪种方式处理参数。未传递的参数不会被自动生成用户变量，使用时仍然是`UNDEFINED`状态，请一定注意。可以通过`IS UNDEFINED`进行判断是否传递了参数。

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
* [io.qross.pql.PQL 类](/pql/class.md)
* [PQL 中的变量](/pql/variable.md)
* [任务调度工具 Keeper](/keeper/overview.md)
* [PQL 执行器 Worker](/worker/overview.md)
* [统一接口 OneApi](/oneapi/overview.md)
* [模板引擎 Voyager](/voyager/overview.md)
# 命令模板

在 [DAG 工作流](/keeper/dag.md)中，已经介绍过 Keeper 中命令 Command 相关的内容。**命令模板 Command Template** 也是 Keeper 特有的概念，目的是为了提高命令的复用性，快速配置调度任务。命令模板的原理是先设置好一个命令模板，然后再设定好相应的参数。在配置调度时，只需要传递相应的参数即可，根据传递的参数不同，即可运行不同的任务。命令模板主要使用 PQL 进行配置，也支持通过嵌入 PQL 配置 Shell 和 Python 脚本。命令模板在 [Master 管理工具](/master/overview.md)中配置，路径为“系统”-“Keeper监控”-[“命令模板”](/master/system/command-template.md)。

## PQL 命令模板

这里举个例子进行说明，假如已经配置的模板如下：

```sql
OPEN $source_database;
    GET # #{source_get_data_sentence};
SAVE TO $destination_database;
    IF $destination_preparatory_sentence != '' THEN
        PREP # #{destination_preparatory_sentence};
    END IF;
    PUT # #{destination_put_data_sentence};
```

这是一个简单的 PQL 模板，功能是将数据从一个数据源转移到另一个数据源。这个模板需要传递以下 5 个参数：

* `source_database` 源数据库连接名称。
* `source_get_data_sentence` 源数据库获取数据的语句，一般为 SQL 语句。
* `destination_database` 目标数据库连接名称。
* `destination_preparatory_sentence` 目标数据库准备语句的名称，如为了防止重复插入数据，在数据转移之前先把目标数据库中可能重复的数据删除。
* `destination_put_data_sentence` 目标数据库插入或更新数据的语句。

待传递的参数通过对象格式 Json 进行配置，如：

```json
{
    "source_database": "oracle.master",
    "source_get_data_sentence": "SELECT name, age, score FROM students",
    "destination_database": "mysql.slave",
    "destination_preparatory_sentence": "TRUNCATE TABLE students",
    "destination_put_data_sentence": "INSERT INTO students (name, age, score) VALUES (?, ?, ?)"
}
```

传递的参数都会自动转成变量，如参数`source_database`会自动转成变量`$source_database`，类型为文本字符串。当然也可能使用基本传参格式`#{source_database}`，这种方式会忽略参数类型。上例通过传递参数给命令模板，就形成了一个基本的两个数据库之间的数据转移的逻辑。通过传递不同的参数，则可以实现其他数据库之间的不同数据表之间的转移逻辑。当然这只是一个特别简单的模板，实际应用中远比这复杂得多，比如增量抽取、大数据量等，这个例子只是为了说明命令模板如何运作的。

## Shell 命令模板

跟 DAG 中 Command 的配置一样，Shell 的模板逻辑中可以嵌入 PQL 以实现更高的灵活性。

```sh
<% IF $mode == 'whole' THEN %>
java -jar student-scores.jar --date #{date}
<% ELSE %>
java -cp student.jar --bulking <%=$task_time GET 'yyyyMMdd'%>
<% END IF %>
```

上例可以传递参数比较少，只有两个：

```json
{
    "mode": "whole",
    "date": "2021-05-04"
}
```

上例中`$task_time`变量是默认传递的参数，其他默认传递的参数见[工作流 DAG](/keeper/dag.md) 中的介绍。

这个模板在运行时，会先处理 PQL 相关的逻辑，生成最终的 Shell 命令，然后再以 Shell 命令运行。[嵌入式 PQL](/pql/embedded.md) 的语法非常简单，语法非常简单，基本上 Jsp 或 Asp 风格的嵌入语法`<%`和`%>`就够用了。

## Python 命令模板

Python 命令模板的处理逻辑与 Shell 命令模板完全相同，只不过核心逻辑由 Shell 改为了 Python，仍由 PQL 作为辅助逻辑。

```python
def main() :
    train_path = '#{train_path}'
    test_path = '<%=$test_path IF UNDEFINED './default_test.csv' %>'
    x_train, y_train, x_test, y_test = load_data(train_path, test_path)
    model(x_train, y_train, x_test, y_test)

if __name__ == '__main__':
    main()
```

上例中传递两个参数：

```json
{
    "train_path": "./mnist_train.csv",
    "test_path": "./mnist_test.csv"
}
```

假设 Python 逻辑是一个机器学习算法，可传入不同的训练集和测试集，从而生成不同的结果，无需要在多个调度命令中配置大量重复的代码。

## 小结

命令模板在本质上也可以理解为是一个“函数”，其目的是减少重复代码的编写和维护，通过传入不同的参数来得到不同的结果。

---
参考链接

* [工作流 DAG](/keeper/dag.md)
* [Voyager 模板引擎](/voyager/overview.md)
* [配置命令模板](/master/system/command-template.md)
* [Master 管理工具](/master/overview.md)
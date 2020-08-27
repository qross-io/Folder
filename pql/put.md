# 将缓冲区的数据保存到数据库 PUT
PUT语句用于将缓冲区的数据更新或保存到目标数据源，由于是批量操作，对于关系型数据库来说，PUT的效率非常高。  
```sql
OPEN mysql.classes;
    GET # SELECT name, score FROM students;
SAVE TO mysql.school;
    PUT # DELETE FROM scores WHERE name='#name';
    PUT # INSERT INTO scores (name, score) VALUES (?, ?);
SAVE TO mysql.grade2;
    PUT # UPDATE scores SET score=#score WHERE name=&name;
```
以上示例中，将GET得到的数据PUT了三次，分别在两个库中执行了删除、插入和更新操作，三次更新都是使用的同样的数据。PUT语句在批量更新时，将数据自动以每1000行分批入库，这个数字经过作者多次测试，可以达到最佳性能。  
PUT语句支持3种格式的数据占位符，分别说明如下：

* 问号`?`：要求PUT语句中问号占位符的数量和位置顺序必须与GET得到的数据的列数和字段顺序一致，自动判断数据类型。如上面示例语句要求第一个问号是`name`，第二个问题是`score`，须与GET得到的数据字段的位置和数量对应。问号占位符是JDBC原生支持的占位符类型。
* 井号`#`+字段名：这种占位符不要求字段顺序和数量对应，但是不会自动判断数据类型，如遇字符串或时间等数据类型，需要自已添加引号。如上面示例中第一条PUT语句和第三条PUT语句。井号占位符由于最简单，所以可以保证最大的灵活性，甚至可以非字段位置输入，如 `PUT # UPDATE table_#name SET score=#score`。这个场景在分库分表时会经常用到。
* 与号`&`+字段名：这种占位符也不要求字段顺序和数量对应，而且会自动判断数据类型，如遇字符串或时间等数据类型，会自动添加单引号。
* 字段名命名规则：因为从数据库返回数据的不确定性，字段名由数据库确定，建议自动名由英文字母开头，可以包含英文字母、数字和下划线。如果碰到不规划字段命令，处理办法参见下一条。
* 因为需要把占位符放在语句中，使用井号占位符和与号占位符有时会遇到字段名与语句中的字符冲突或者字段名不规范的问题，如 `UPDATE table SET name='#name_123', value='#分数'`，这种情况下只能识别成`#name_123`，不能正确识别成`#name`和`#分数`。解决办法是在字段名两端加小括号，修改成`UPDATE table SET name='#(name)_123', value='#(分数)'`。虽然PQL提供了解决方案，如非特别需要，仍建议在SELECT语句中使用AS将字段名命名为英文。
* 如果声明的占位符名称不在缓冲区的字段列表中，则这个占位符将保持原样，所以请仔细检查字段名。
* 如果要在语句中输入的井号`#`和与号`&`与变量名冲突时，请使用[特殊字符的编码](/pql/characters.md)替换，其中`#`替换为`~u0023`，`&`替换为`~u0026`。
  
### INSERT语句的串接模式
在有的数据库中，如MySQL，支持在INSERT语句中的VALUES后面附加多组值，用这种方式将数据一次性插入到数据库，例如：
```sql
INSERT INTO students (name, score) VALUES ('Tom', 89), ('Jerry', 77), ('Ted', 91);
```
这种操作在大数据量下会极大的提高插入性能，PUT语句也支持设置这种串接操作。语法如下：
```sql
PUT # INSERT INTO students (name, score);
```
是的，不需要输出VALUES及后面的内容，但要求字段顺序和数量一致，可以理解为是省略了问号点位符的INSERT语句。

### 与PUT有关的全局变量
* `@AFFECTED_ROWS_OF_LAST_PUT` 最后一次PUT影响目标数据库的行数。
* `@TOTAL_AFFECTED_ROWS_OF_RECENT_PUT` 最近所有PUT影响目标数据库的行数。

### PUT语句的多线程版本BATCH语句
如果获取数据使用的是PAGE或BLOCK语句，需要将百万级甚至千万级的数据流转到目标数据库，由于PUT是单线程的，性能很难满足如此大的数据量要求。这种场景下可以使用 **[BATCH语句](/pql/batch.md)** 代替PUT。

---
参考链接

* [打开和切换数据源 OPEN](/pql/open.md)
* [跨数据源数据流转 SAVE](/pql/save.md)
* [将数据保存在缓冲区 GET](/pql/get.md)
* [在目标数据源执行非查询操作 PREP](/pql/prep.md)
* [缓冲区数据再查询 PASS](/pql/pass.md)
* [中间缓存内存数据库 CACHE](/pql/cache.md)
* [中间临时文件数据库 TEMP](/pql/temp.md)
* [大数据量下的分页查询 PAGE](/pql/page.md)
* [大数据量下的分块查询和更新 BLOCK](/pql/block.md)
* [大数据量下的批量更新 BATCH](/pql/batch.md)
* [全局变量 @](/pql/global.md)
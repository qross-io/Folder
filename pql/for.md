# 循环遍历 FOR语句
FOR语句用来遍历集合对象，集合对象可以是数组、数据行或数据表。由于PQL本身的语言特点，在数据开发中循环用得非常少，好多工作都被GET语句代替了。不过在[Voyager模板引擎](/doc/vayager/overview)中，由于要向页面上输出数据，会经常用到。
```
FOR $i IN 1 TO 10 LOOP
    PRINT $i;
    INSERT INTO students (idx) VALUES ($i);
END LOOP; 
```
上例中会遍历从`1`到`10`，包含`10`。注意`END LOOP`后面必须有分号，表示FOR语句结束。上例仅是说明FOR语句的用法，实例开发中不建议用这种方式插入数据，用[GET语句](/doc/pql/get)和[PUT语句](/doc/pql/put)会提高性能。
```
GET # 1 TO 10;
PUT # INSERT INTO students (idx);
```
FOR语句遍历数组的其他示例（后面的示例都省略了`LOOP`后的部分）。
```
FOR $i IN 1 UNTIL 10 LOOP  -- 遍历从1到9，不包含10
FOR $i IN 1 TO 10 REVERSE LOOP -- 遍历从10到1
FOR $i IN 10 TO 1 LOOP  -- 效果同上
FOR $i IN [1, 2, 3, 4, 5, 6, 7] LOOP
FOR $key IN (REDIS KEYS *) LOOP
FOR $score IN (SELECT score FROM students -> FIRST COLUMN) LOOP
```
上例后面两句是从数据源中查询结果，语句两端必须加括号。后一句因为变量字段只有一个`$score`，且第一列就是想要的结果，所以`-> FIRST COLUMN`可以省略掉。下面看一下遍历数据表的例子，一般数据表都是从数据源查询得来：
```
FOR $a, $b, $c IN (SELECT a, b, c FROM table1) LOOP --遍历查询结果数据表
FOR $name, $score IN (SELECT name, score FROM students -> INSERT { "name": "Tom", "score": 89}) LOOP -- 遍历编辑后的查询结果表
FOR $name, $age, $height IN @BUFFER LOOP -- 遍历缓冲区数据
```
在遍历数据表时，可以理解为将数据表的每一行进行析构赋值（见[SET语句](/doc/pql/set)），把每行每个字段位置对应的值赋值给循环变量。

最后看一下遍历数据行的例子。
```
FOR $key, $value IN { "name": "Tom", "age": 18 } LOOP
FOR $field, $value IN (SELECT name, score, rate FROM students -> FIRST ROW) LOOP
```
特别注意，遍历数据行可以理解为把数据行转成一个两列的表格，这两列分别是数据行的字段名和字段值。上例中第一句，得到两次遍历，每一次遍历`$key`和`$value`分别为`name`和`Tom`，第二次遍历`$key`和`$value`分别为`age`和`18`。

#### FOR-OF循环
就像有SET语句也有VAR语句一样，FOR语句除了`FOR-IN`循环外，还有`FOR-OF`循环。在遍历数组时，`FOR-IN`循环和`FOR-OF`循环结果一致，但在遍历数据行和数据表时有所不同。
```
FOR $item OF { "name": "Tom", "age": 18 } LOOP
FOR $student OF (SELECT name, score, rate FROM students) LOOP
FOR $row OF @BUFFER LOOP
```
`FOR-OF`循环只能声明一个循环变量，这个变量在遍历数据行和数据表时，其数据类型是数据行。可以理解为循环变量没有经过析构赋值，将集合中的整个项赋给了循环变量（原理同[VAR语句](/doc/pql/var)）。`FOR-IN`遍历数据表时需要预先知道数据表中的列数，`FOR-OF`循环在不知道数据表有多少列时或列非常多时非常有用。在循环体中，通过数据行类型点规则访问数据行某项的值，如`$student.name`或`$student.score`。也可以通过`UNDEFINED`对是否包含某个字段进行判断，如`IF $student.rate IS UNDEFINED THEN`。

#### 循环变量的作用域
循环变量的值根据很一次循环都可能不同，其作用域仅限制在循环内部，循环体外面是访问不到的。循环退出后，循环变量即消失。如果定义的循环变量的变量名跟循环体外面的变量名一样时，则会在循环体内部访问不到外面同名变量的值，所以不建议将循环变量命名成与循环体外变量相同的名字。

#### 循环的继续和中断
PQL提供了[EXIT语句](/doc/pql/exit)和[CONTINUE语句](/doc/pql/continue)用于中断和继续下一次循环，点击链接查看详细内容。

---
参考链接
* [退出循环 EXIT](/doc/pql/exit)
* [继续下一次循环 CONTINUE](/doc/pql/continue)
* [条件循环 WHILE](/doc/pql/while)
* [将数据保存在缓冲区 GET](/doc/pql/get)
* [将缓冲区的数据保存到数据库 PUT](/doc/pql/put)
* [Sharp表达式](/doc/pql/sharp)
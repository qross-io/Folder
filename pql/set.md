# 变量声明 SET语句
在PQL中，SET语句一般用于基本数据类型变量的声明和赋值。
```
SET $a := 1;
SET $b := "hello";
SET $i := 1 + 2;
SET $s := 'a' + $t;
SET $s := 'str1' CONCAT 'str2';
SET $affected := UPDATE table SET a=1 WHERE a<>1;
```
赋值表达式使用赋值符号`:=`连接两端（和Oracle相同），SET语句中右侧值或表达式不可省略，就是说声明的变量必须赋值，否则会出现解析错误。如果想只声明不赋值，可使用[VAR语句](/doc/pql/var)。

#### 析构赋值
有时我们需要从SQL语句中提取值或者一次赋多个值。SET支持析构赋值。
```
SET $name, $score := SELECT name, score FROM table1 LIMIT 1;
SET $x, $y, $z := [1, 2, 3];
```
上面示例第一条语句中，将SELECT查询结果字段`name`和`score`的值分别赋给变量`$name`和`$score`。如果SELECT语句返回的数据大于1行，那么将会把最后一行的值赋值给变量。  
上面示例第二行语句中，变量`$x`、`$y`、`$z`的值分别为`1`、`2`和`3`。  
析构异常的情况，如变量名的数量多于可赋的变量值的数量，如
```
SET $x, $y, $z := [1, 2];
SET $name, $score, $age := SELECT name, score FROM table1;
```
这种情况下变量`$z`和变量`$age`将会赋值为`NULL`。所以 `SET $a := [];`，这个赋值无意义，变量`$a`将得到`NULL`。
如果变量名的数量少于变量值的数量，则多余的值将被忽略。在FOR-IN循环的变量赋值中，使用相同的规则。

#### 复合数据类型赋值
一般情况下，SET语句只能用于基本数据类型的赋值，因为支持析构赋值，所以SET语句会自动解析复合结构。如果需要要把复合类型赋值给变量该怎么办？PQL提供了两种方法，一种方法是改变赋值符号，见下面示例，另一种方法是使用[VAR语句](/doc/pql/var)。
```
SET $array =: [1, 2, 3];
SET $row =: SELECT name, score FROM table1 ->　FIRST ROW;
```
将赋值符号由`:=`改成`=:`可以实现将右侧复合结构的数据赋值给变量，上例中变量`$array`得到一个数组，`$row`得到一个数据行。提供这种赋值方式的原因是因为有的开发人员认为，VAR语句一般用于声明变量，而SET语句用于更新变量，由于PQL不可省略语句关键字，而VAR语句用于更新变量会很奇怪，所以有了这种解决方案。

---
参考链接
* [数据类型](/doc/pql/datatype)
* [变量声明 VAR](/doc/pql/var)
* [循环控制 FOR](/doc/pql/for)
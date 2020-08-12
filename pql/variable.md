# PQL中的变量
与其他语言不同，PQL中的变量除了可以进行运算之外，还可以和SQL语句写在一起，作为其他语句的一部分，解释器会在执行时替换成其对应的值并自动判断数据类型。
```
SET $hello := 'World';
PRINT $hello;

SET $id := 1;
SELECT * FROM table1 WHERE id=$id;
```
**用户变量必须以美元`$`字符开头，变量名称只能由字母、数字和下划线组成。变量名不区分大小写（`$user`和`$User`是同一个变量），但英文字母建议全部小写。** 声明变量不需要也不能指定类型，解释器会自动推断。PQL中没有常量的概念，所有变量均可更新。

#### 变量运算
变量除了可以嵌入到语句中之外，也可以进行各种运算。
```
-- 数字加和
SET $i := $i + 1;
-- 字符串连接
SET $str := $hello + ' World';
-- 其他算术运算
SET $a := 2 * 6;
SET $b := $a / 4;
SET $c := $b - $a;
SET $d := 100 % $a; -- 取余
-- Sharp表达式
SET $upper := $str TO UPPER; -- 将字符串转成大写
SET $n := $d POW 3; -- 3次方
```

#### 变量作用域
变量有自己的作用域和生命周期。如下示例：
```
SET $d := 2;
FOR $i IN 1 TO 10 LOOP
    SET $x := $i % 2;
    IF $x != 0 THEN
        SET $y := $x * $i;
        PRINT $y;
    ELSE
        SET $z := $x + $i;
        PRINT $z;
    END IF;
END LOOP;
```
上面例子中：变量`$d`的作用域为整个PQL过程; 变量`$i`和变量`$x`的作用域在`FOR`循环中; 变量`$y`的作用域仅在`IF`语句块内，更确切点是在`THEN`和`ELSE`之间; 变量`$z`的作用域在`ELSE`和`END IF`中间。同样的，函数参数和在函数体内声明的变量作用域也仅在函数内。  

#### 变量的状态
PQL使用几个关键词来表示变量的状态，可用于条件判断，使用时不区分大小写。
* `UNDEFINED` 表示变量未声明，或不在作用域内，一般用于判断。
```
IF $a IS UNDEFINED THEN
    ...
END IF;
```
已嵌入到语句中的未声明的变量在计算时将恢复为`UNDEFINED`。
* `NULL` 表示变量已声明未赋值，也可以给变量直接赋值`NULL`，但一般不会这么做。
```
VAR $a;
SET $b, $c := [1]; -- $b的值为1, $c 的值为NULL
SET $d := NULL;
```
* `EMPTY` 表示变量是否为空，比如字符串是否为空、集合变量是否为空等。
> 数据表为空表示数据表中没有任何数据行。  
> 数据行为空表示数据行中没有任何字段。  
> 数组为空表示数组中没有任何元素。  
> 字符串为空表示字符串中没有任何字符。
```
IF $str IS EMPTY THEN
    ......
END IF;
```

**数据类型的判断见[条件表达式](/doc/pql/condition)**

#### 将变量嵌入到语句中
变量可以嵌入到语句当中，如本页第一个例子，解释器会自动判断类型。如果碰到变量名与语句冲突不能正确识别时，可以在变量名上加上小括号; 如果是字符串变量，在解析时会自动添加引号，如果不想让解释器自动添加引号，可以变量末尾加叹号`!`。
```
SET $group := 'students';
SET $table := '''$(group)_scores'''; -- 富字符串
GET # SELECT * FROM $table! WHERE score>=60;
```
上例中，如果变量`$group`引用时不加小括号，就会识别为`$group_scores`; 如果变量`$table`在引用时不加叹号，则会把语句解析为`SELECT * FROM 'students_scores' WHERE score>=60`，执行时会出现语法错误。
重复一句，如果嵌入到语句中的变量未声明或书写错误，会被恢复为`UNDEFINED`，很可能会造成语句出错。

---
参考链接
* [数据类型](/doc/pql/datatype)
* [变量声明 SET](/doc/pql/set)
* [变量声明 VAR](/doc/pql/var)
* [全局变量 @](/doc/pql/global)
* [富字符串](/doc/pql/rich)
* [条件表达式](/doc/pql/condition)
* [条件判断 IF](/doc/pql/if)
* [循环控制 FOR](/doc/pql/for)

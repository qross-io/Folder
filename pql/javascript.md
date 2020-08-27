# PQL中的Javascript

这里提到的Javascript并不涉及任何前端代码，更不涉及任何DOM操作。PQL中的基础运算由Javascript的逻辑实现，所有在PQL中与Javascript关系非常密切。这个章节对于使用PQL没有任何影响，仅作了解即可。如果你会一些Javascript，这章对你来说会非常容易理解。

```sql
SET $a := 12 * 5;  -- 基本运算
SEt $str := 'hello ' + 'world'; -- 字符串连接
SET $m := Math.pow(2, 3);  -- 数学运算
PRINT $str.substring(0, 3); -- 字符串操作
IF /^hello/i.test($str) THEN ... -- 正则表达式和条件判断
SELECT * FROM table1 WHERE create_time>=${ '20200818' CONCAT '18:09:20'.replace(':', '') }!; -- 混合运算
```

在PQL上，Javascript基础运算的优先级最高，会先计算完基本表达式再进行其他运算。不过PQL中的Javascript只是计算表达式，对语句和函数等并不提供解析。Java 1.8对ES6等高版本的Javascript的语法均不支持，也许后续版本会有改进。

---
参考链接

* [在PQL中调用Java方法 INVOKE](/pql/invoke.md)
* [在PQL中运行Shell命令 RUN](/pql/run.md)
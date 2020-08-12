# 简单输出 ECHO语句
ECHO语句和OUTPUT语句](/doc/pql/output)一样，都用来输出和返回PQL过程的计算结果。
```htm
ECHO <h1>标题1</h1>;
ECHO <div>${name}</div>;
ECHO <div># description #</div>;
```
* [OUTPUT语句](/doc/pql/output)支持完整的规则，后面的表达式和语句都会被计算。ECHO语句只计算嵌入式变量（上例`${name}`）和多语句言占位符（上例`# description #`），其他内容不作处理，直接返回。
* ECHO语句后面忽略数据类型，均为字符串。
* ECHO语句属于显示输出，结果都会被输出和返回。
* ECHO语句是嵌入式PQL的基础，应用于模板引擎Voyager。


ECHO语句另外一种应用场景是语句占位。
```
ECHO;
```
这条语句什么都不做，没有任何功能，仅是一条语句。

---
参考链接
* [数据输出 OUTPUT](/doc/pql/output)
* [数据类型](/doc/pql/datatype)
* [模板引擎 Voyager](/doc/voyager/overview)
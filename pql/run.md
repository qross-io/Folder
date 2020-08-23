# 运行Shell命令 RUN

RUN语句用来运行Shell命令，属于PQL跟其他语言交互的一部分。示例：
```sql
RUN SHELL "ls";
```
RUN语句现在的功能还比较简单，仅是运行命令并在控制台打印Shell命令的输出内容。比如返回值等暂时不能保存和处理，将来会加入更多功能。


---
参考链接
* [在PQL中执行Java静态方法](/pql/invoke.md)
* [PQL基本语法](/pql/basic.md)
* [PQL中的Javascript](/pql/javascript.md)
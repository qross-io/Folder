# 退出程序 EXIT CODE语句
EXIT CODE语句用于退出当前进程，中断整个PQL过程的执行。
```sql
EXIT CODE 0;
EXIT CODE -1;
```
其中`0`为正常退出，非`0`为异常退出。

注意：在OneApi接口开发和Keeper集成开发中千万不要使用，会退出整个Spring Boot项目或退出Keeper。在数据计算脚本开发中可以使用。


---
参考链接

* [中断循环 EXIT](/pql/exit.md)
* [休息一下 SLEEP](/pql/sleep.md)
* [统一接口 OneApi](/oneapi/overview.md)
* [调度工具 Keeper](/keeper/overview.md)
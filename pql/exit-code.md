# 退出程序 EXIT CODE 语句

EXIT CODE 语句用于退出当前进程，中断整个 PQL 过程的执行。

```sql
EXIT CODE 0;
EXIT CODE -1;
```

其中`0`为正常退出，非`0`为异常退出。

注意：在 OneApi 接口开发和 Keeper 集成开发中千万不要使用，会退出整个 Spring Boot 项目或退出 Keeper。在数据计算脚本开发中可以使用。


---
参考链接

* [中断循环 EXIT](/pql/exit.md)
* [休息一下 SLEEP](/pql/sleep.md)
* [统一接口 OneApi](/oneapi/overview.md)
* [调度工具 Keeper](/keeper/overview.md)
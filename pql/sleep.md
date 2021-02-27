# 休息一下 SLEEP 语句

SLEEP 语句可以让当前进程挂起（暂停）一段时间，常用于循环中。

```sql
SLEEP 100;  -- 默认毫秒
SLEEP 50 MILLIS; -- 毫秒
SLEEP 1 MILLI; -- 毫秒
SLEEP 1 SECOND; -- 秒
SLEEP 2 SECONDS; -- 秒
SLEEP 1 MINUTE; -- 分钟
SLEEP 2 MINUTES; -- 分钟
SLEEP TO NEXT SECOND; -- 下一（整）秒
SLEEP TO NEXT MINUTE; -- 下一（整）分钟
```

---
参考链接

* [循环控制 FOR](/pql/for.md)
* [条件循环 WHILE](/pql/while.md)
* [退出程序 EXIT CODE](/pql/exit-code.md)
# 休息一下 SLEEP语句
SLEEP语句可以让当前进程挂起（暂停）一段时间，常用于循环中。
```
SLEEP 100;  -- 默认毫秒
SLEEP 50 MILLIS; -- 毫秒
SLEEP 1 MILLISECOND; -- 毫秒
SLEEP 30 MILLISECONDS; -- 毫秒
SLEEP 1 SECOND; -- 秒
SLEEP 2 SECONDS; -- 秒
SLEEP 1 MINUTE; -- 分钟
SLEEP 2 MINUTES; -- 分钟
SLEEP TO NEXT SECOND; -- 下一（整）秒
SLEEP TO NEXT MINUTE; -- 下一（整）分钟
```

---
参考链接
* [循环控制 FOR](/doc/pql/for)
* [条件循环 WHILE](/doc/pql/while)
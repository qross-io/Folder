# MySQL 函数

## 字符串函数

1. `ASCII(str)` 返回值为字符串 str 的最左字符的数值，即取得最左字符的 ASCII 码。假如 str 为空字符串，则返回值为`0`。假如 str 为`NULL`，则返回值为`NULL`。 ASCII() 用于带有从`0`到`255`的数值的字符。

2. `BIN(N)` 返回值为 N 的二进制值的字符串表示，即转为二进制。其中 N 为一个 long (BIGINT) 数字。这等同于`CONV(N,10,2)`。假如 N 为`NULL`，则返回值为`NULL`。

3. `BIT_LENGTH(str)` 返回值为二进制的字符串 str 长度。

4. `CHAR(N,... [USING charset])` 将每个参数 N 理解为一个整数，其返回值为一个包含这些整数的代码值所给出的字符的字符串。`NULL`值被省略。即将所有参数转为字符后连接在一起。

5. `CHAR_LENGTH(str)` 返回值为字符串 str 的长度，长度的单位为字符。

6. `CHARACTER_LENGTH(str)` 是 `CHAR_LENGTH()` 的同义词。

7. `COMPRESS(string_to_compress)` 压缩一个字符串。

8. `CONCAT(str1, str2, ...)` 返回结果为连接参数产生的字符串。

9. `CONCAT_WS(separator, str1, str2,...)` CONCAT_WS() 代表 CONCAT with Separator ，是 CONCAT() 的特殊形式。第一个参数是其它参数的分隔符。分隔符的位置放在要连接的两个字符串之间。分隔符可以是一个字符串，也可以是其它参数。如果分隔符为`NULL`，则结果为`NULL`。函数会忽略任何分隔符参数后的`NULL`值。

10. `CONV(N,from_base,to_base)` 不同数基间转换数字。返回值为数字的 N 字符串表示，由 from_base 基转化为 to_base 基。如有任意一个参数为`NULL`，则返回值为`NULL`。自变量 N 被理解为一个整数，但是可以被指定为一个整数或字符串。最小基数为`2` ，而最大基数则为`36`。

11. `ELT(N,str1,str2,str3,...)` 若 N = 1，则返回值为 str1 ，若 N = 2，则返回值为 str2 ，以此类推。若 N 小于`1`或大于参数的数目，则返回值为`NULL`。

12. `EXPORT_SET(bits, on, off[, separator[, number_of_bits]])` 返回值为一个字符串，其中对于 bits 值中的每个位组，可以得到一个 on 字符串，而对于每个清零比特位，可以得到一个off 字符串。bits 中的比特值按照从右到左的顺序接受检验 (由低位比特到高位比特)。字符串被分隔字符串分开(默认为逗号`,`)，按照从左到右的顺序被添加到结果中。number_of_bits 会给出被检验的二进制位数 (默认为 64)。

13. `FIELD(str, str1, str2, str3, ...)`  返回值为 str1, str2, str3, ~ 列表中的 str 指数（位置）。在找不到 str 的情况下，返回值为`0`。如果所有对于 FIELD() 的参数均为字符串，则所有参数均按照字符串进行比较。如果所有的参数均为数字，则按照数字进行比较。否则，参数按照双倍进行比较。

14. `FIND_IN_SET(str, strlist)` 分析逗号分隔的 list 列表，如果发现 str，返回 str 在 list 中的位置。假如字符串 str 在由 N 子链组成的字符串列表 strlist 中， 则返回值的范围在 1 到 N 之间（即 str 在 strlist 中的位置）。一个字符串列表就是一个由一些被`,`符号分开的自链组成的字符串。

15. `FORMAT(X,D)` 将数字 X 的格式写为'#,###,###.##',以四舍五入的方式保留小数点后 D 位， 并将结果以字符串的形式返回。若 D 为 0, 则返回结果不带有小数点，或不含小数部分。

16. `HEX(N_or_S)` 如果 N_OR_S 是一个数字，则返回一个 十六进制值 N 的字符串表示，在这里，N 是一个 long (BIGINT)数。这相当于 CONV(N,10,16)。

17. `INSERT(str, pos, len, newstr)` 将字符串 str 从第 x 位置开始，y 个字符长的子串替换为字符串 instr，返回结果。str 中的字符被 newstr 替换。返回字符串 str, 其子字符串起始于 pos 位置和长期被字符串 newstr 取代的 len 字符。 如果 pos 超过字符串长度，则返回值为原始字符串。 假如 len 的长度大于其它字符串的长度，则从位置 pos 开始替换。若任何一个参数为`NULL`，则返回值为`NULL`。

18. `INSTR(str, substr)` 返回字符串 str 中子字符串的第一个出现位置。这和 LOCATE() 的双参数形式相同，除非参数的顺序被颠倒。

19. `LCASE(str)` 是 LOWER() 的同义词。返回将字符串 str 中所有字符改变为小写后的结果。

20. `LEFT(str, len)` 返回从字符串 str 开始的 len 最左字符。

21. `LENGTH(str)` 返回值为字符串 str 的长度，单位为字节。一个多字节字符算作多字节。这意味着对于一个包含 5 个 2 字节字符的字符串，LENGTH() 的返回值为 10, 而 CHAR_LENGTH() 的返回值则为 5。

22. `LOAD_FILE(file_name)` 读取文件并将这一文件按照字符串的格式返回。 
    ```sql
    UPDATE tbl_name
           SET blob_column=LOAD_FILE('/tmp/picture')
           WHERE id=1;
    ```

23. `LOCATE(substr, str)` 或 `LOCATE(substr, str, pos)` 第一个语法返回字符串 str 中子字符串 substr 的第一个出现位置。第二个语法返回字符串 str 中子字符串 substr 的第一个出现位置, 起始位置在 pos。如若 substr 不在 str中，则返回值为 0。

24. `LOWER(str)` 返回字符串 str 以及所有根据最新的字符集映射表变为小写字母的字符 (默认为 cp1252 Latin1)。

25. `LPAD(str, len, padstr)` 返回字符串 str, 其左边由字符串 padstr 填补到len 字符长度。假如 str 的长度大于 len, 则返回值被缩短至 len 字符。即在 str 前面添加长度为 len 的 padstr.

26. `LTRIM(str)`  返回字符串 str ，其引导空格字符被删除。

27. `MAKE_SET(bits, str1, str2, ...)` 返回一个设定值 (一个包含被`,`号分开的字字符串的字符串) ，由在 bits 组中具有相应的比特的字符串组成。str1 对应比特 0, str2 对应比特 1，以此类推。str1, str2, ...中的`NULL`值不会被添加到结果中。

28. `MID(str, pos, len)` 是 SUBSTRING(str, pos, len) 的同义词。

29. `OCT(N)` 返回一个 N 的八进制值的字符串表示，其中 N 是一个long (BIGINT)数。这等同于 CONV(N,10,8)。若 N 为`NULL`，则返回值为`NULL`。

30. `OCTET_LENGTH(str)` 是 LENGTH() 的同义词。

31. `ORD(str)` 若字符串 str 的最左字符是一个多字节字符，则返回该字符的代码，假如最左字符不是一个多字节字符，那么 ORD() 和函数 ASCII() 返回相同的值。

32. `POSITION(substr IN str)` 是 LOCATE(substr, str)同义词。返回子串 substr 在字符串 str 中第一次出现的位置。

33. `QUOTE(str)` 引证一个字符串，由此产生一个在SQL语句中可用作完全转义数据值的结果。即用反斜杠转义 str 中的单引号。

34. `REPEAT(str, count)` 返回一个由重复的字符串 str 组成的字符串，字符串 str 的数目等于 count。 若 count <= 0，则返回一个空字符串。若 str 或 count 为`NULL`，则返回`NULL`。

35. `REPLACE(str, from_str, to_str)` 返回字符串 str 以及所有被字符串 to_str 替代的字符串 from_str 。

36. `REVERSE(str)` 返回字符串 str ，顺序和字符顺序相反。

37. `RIGHT(str, len)` 从字符串 str 开始，返回最右 len 字符。

38. `RPAD(str, len, padstr)` 返回字符串 str, 其右边被字符串 padstr 填补至 len 字符长度。假如字符串 str 的长度大于 len，则返回值被缩短到与 len 字符相同长度。

39. `RTRIM(str)` 返回字符串 str ，结尾空格字符被删去。

40. `SOUNDEX(str)` 从 str 返回一个 soundex 字符串。

41. `SPACE(N)` 返回一个由 N 间隔符号组成的字符串。

42. `SUBSTRING(str, pos)` 或 `SUBSTRING(str FROM pos)` 或 `SUBSTRING(str, pos, len)` 或 `SUBSTRING(str FROM pos FOR len)` SUBSTR() 是 SUBSTRING() 的同义词。不带有 len 参数的格式从字符串 str 返回一个子字符串，起始于位置 pos。带有 len 参数的格式从字符串 str 返回一个长度同 len 字符相同的子字符串，起始于位置 pos。使用 FROM 的格式为标准 SQL 语法。也可能对 pos 使用一个负值。假若这样，则子字符串的位置起始于字符串结尾的 pos 字符，而不是字符串的开头位置。

43. `SUBSTRING_INDEX(str,delim,count)` 在定界符 delim 以及 count 出现前，从字符串 str 返回自字符串。若 count 为正值，则返回最终定界符(从左边开始)左边的一切内容。若 count 为负值，则返回定界符（从右边开始）右边的一切内容。

44. `TRIM([{BOTH | LEADING | TRAILING} [remstr] FROM] str)` 或 `TRIM(remstr FROM] str)` 返回字符串 str， 其中所有 remstr 前缀和/或后缀都已被删除。若分类符 BOTH、LEADIN 或 TRAILING 中没有一个是给定的，则假设为 BOTH。 remstr 为可选项，在未指定情况下，可删除空格。

45. `UCASE(str)` UCASE() 是 UPPER() 的同义词。返回将字符串 str 中所有字符转变为大写后的结果。

46. `UNCOMPRESS(string_to_uncompress)` 对经 COMPRESS() 函数压缩后的字符串进行解压缩。

47. `UNCOMPRESSED_LENGTH(compressed_string)` 返回压缩字符串压缩前的长度。

48. `UNHEX(str)` 执行从 HEX(str) 的反向操作。就是说，它将参数中的每一对十六进制数字理解为一个数字，并将其转化为该数字代表的字符。结果字符以二进制字符串的形式返回。

49. `UPPER(str)` 返回字符串 str， 以及根据最新字符集映射转化为大写字母的字符 (默认为cp1252 Latin1).

50. `STRCMP(expr1,expr2)` 若所有的字符串均相同，则返回`0`，若根据当前分类次序，第一个参数小于第二个，则返回`-1`，其它情况返回`1`。
 
MySQL 必要时自动变换数字为字符串，并且反过来也如此：

```sql
SELECT 1 + "1"; -- 2
SELECT CONCAT(2, ' test'); -- '2 test'
```

如果你想要明确地变换一个数字到一个字符串，把它作为参数传递到 CONCAT()。如果字符串函数提供一个二进制字符串作为参数，结果字符串也是一个二进制字符串。被变换到一个字符串的数字被当作是一个二进制字符串。

## 日期时间函数

1. `DAYOFWEEK(date)` 返回日期date是星期几(1=星期天, 2=星期一, ~7=星期六, ODBC标准)
    ```sql
    select DAYOFWEEK('1998-02-03'); -- 3
    ```

2. `WEEKDAY(date) ` 返回日期 date 是星期几 (0=星期一,1=星期二,~6= 星期天)。 
    ```sql
    select WEEKDAY('1997-10-04 22:23:00'); -- 5 
    select WEEKDAY('1997-11-05');  -- 2
    ```

3. `DAYOFMONTH(date)` 返回 date 是一月中的第几日(在 1 到 31 范围内) 
    ```sql
    select DAYOFMONTH('1998-02-03');  -- 3 
    ```

4. `DAYOFYEAR(date)` 返回date是一年中的第几日(在1到366范围内) 
    ```sql
    select DAYOFYEAR('1998-02-03'); -- 34
    ```

5. `MONTH(date)` 返回date中的月份数值 
    ```sql
    select MONTH('1998-02-03'); -- 2
    ```

6. `DAYNAME(date)` 返回 date 是星期几(按英文名返回)
    ```sql
    select DAYNAME("1998-02-05"); -- 'Thursday' 
    ```

7. `MONTHNAME(date)` 返回 date 是几月(按英文名返回) 
    ```sql
    select MONTHNAME("1998-02-05"); -- 'February' 
    ```

8. `QUARTER(date)` 返回 date 是一年的第几个季度 
    ```sql
    select QUARTER('98-04-01'); -- 2 
    ```

9. `WEEK(date,first)` 返回 date 是一年的第几周（first 默认值 0, first 取值 1 表示周一是周的开始，0 从周日开始）
    ```sql
    select WEEK('1998-02-20'); -- 7 
    ```

10. `YEAR(date)` 返回 date 的年份（范围在 1000 到 9999）
    ```sql
    select YEAR('98-02-03'); -- 1998 
    ```

11. `HOUR(time)`  返回 time 的小时数（范围是 0 到 23）
    ```sql
    select HOUR('10:05:03'); -- 10 
    ```

12. `MINUTE(time)` 返回 time 的分钟数（范围是 0 到 59） 
    ```sql
    select MINUTE('98-02-03 10:05:03'); -- 5 
    ```

13. `SECOND(time)` 返回 time 的秒数（范围是 0 到 59）
    ```sql
    select SECOND('10:05:03'); -- 3 
    ```

14. `PERIOD_ADD(P,N)` 增加 N 个月到时期 P 并返回（P 的格式 YYMM 或 YYYYMM）
    ```sql
    select PERIOD_ADD(9801,2); -- 199803 
    ```

15. `PERIOD_DIFF(P1, P2)` 返回在时期 P1 和 P2 之间月数（P1 和 P2 的格式 YYMM 或 YYYYMM）
    ```sql
    select PERIOD_DIFF(9802,199703); -- 11 
    ```

16. `TIMESTAMPDIFF(SECOND|MINUTE, time1, time2)`
    ```sql
    select TIMESTAMPDIFF(MINUTE, '2017-08-09 17:00:00', '2017-08-09 18:00:00');
    ```
17. `DATE_ADD(date, INTERVAL expr type)`、`DATE_SUB(date, INTERVAL expr type)`、`ADDDATE(date, INTERVAL expr type)`、`SUBDATE(date, INTERVAL expr type)` 对日期时间进行加减法运算，ADDDATE() 和 SUBDATE() 是 DATE_ADD() 和 DATE_SUB() 的同义词。date 是一个 DATETIME 或 DATE 值，expr 对 date 进行加减法的一个表达式字符串 type 指明表达式 expr 应该如何被解释。type 值含义和期望的 expr格式：
    
    + SECOND 秒 SECONDS
    + MINUTE 分钟 MINUTES
    + HOUR 时间 HOURS
    + DAY 天 DAYS
    + MONTH 月 MONTHS
    + YEAR 年 YEARS
    + MINUTE_SECOND 分钟和秒 "MINUTES:SECONDS"
    + HOUR_MINUTE 小时和分钟 "HOURS:MINUTES"
    + DAY_HOUR 天和小时 "DAYS HOURS"
    + YEAR_MONTH 年和月 "YEARS-MONTHS"
    + HOUR_SECOND 小时, 分钟， "HOURS:MINUTES:SECONDS"
    + DAY_MINUTE 天, 小时, 分钟 "DAYS HOURS:MINUTES"
    + DAY_SECOND 天, 小时, 分钟, 秒 "DAYS HOURS:MINUTES:SECONDS"

    expr 中允许任何标点做分隔符，如果所有是 DATE 值时结果是一个 DATE 值，否则结果是一个 DATETIME 值。如果 type 关键词不完整，则 MySQL 从右端取值，DAY_SECOND 因为缺少小时分钟等于 MINUTE_SECOND。如果增加 MONTH、YEAR_MONTH 或 YEAR，天数大于结果月份的最大天数则使用最大天数。

    ```sql
    SELECT "1997-12-31 23:59:59" INTERVAL 1 SECOND; -- 1998-01-01 00:00:00 
    SELECT INTERVAL 1 DAY "1997-12-31"; -- 1998-01-01 
    SELECT "1998-01-01" - INTERVAL 1 SECOND; -- 1997-12-31 23:59:59 
    SELECT DATE_ADD("1997-12-31 23:59:59",INTERVAL 1 SECOND); -- 1998-01-01 00:00:00 
    SELECT DATE_ADD("1997-12-31 23:59:59",INTERVAL 1 DAY); -- 1998-01-01 23:59:59 
    SELECT DATE_ADD("1997-12-31 23:59:59",INTERVAL "1:1" MINUTE_SECOND); -- 1998-01-01 00:01:00 
    SELECT DATE_SUB("1998-01-01 00:00:00",INTERVAL "1 1:1:1" DAY_SECOND); -- 1997-12-30 22:58:59 
    SELECT DATE_ADD("1998-01-01 00:00:00", INTERVAL "-1 10" DAY_HOUR); -- 1997-12-30 14:00:00
    SELECT DATE_SUB("1998-01-02", INTERVAL 31 DAY); -- 1997-12-02 
    SELECT EXTRACT(YEAR FROM "1999-07-02"); -- 1999 
    SELECT EXTRACT(YEAR_MONTH FROM "1999-07-02 01:02:03"); -- 199907 
    SELECT EXTRACT(DAY_MINUTE FROM "1999-07-02 01:02:03"); -- 20102 
    ```

18. `TO_DAYS(date)` 返回日期 date 是西元 0 年至今多少天，不计算 1582 年以前。
    ```sql
    select TO_DAYS(950501); --728779 
    select TO_DAYS('1997-10-07'); --729669 
    ```

19. `FROM_DAYS(N)` 给出西元 0 年至今多少天返回 DATE 值，不计算1582年以前。
    ```sql
    select FROM_DAYS(729669); -- '1997-10-07' 
    ```

20. `DATE_FORMAT(date, format)` 根据 format 字符串格式化 date 值。
    在 format 字符串中可用标志符：
    + `%M` 月名字，January ~ December
    + `%W` 星期名字，Sunday ~ Saturday
    + `%D` 有英语前缀的月份的日期，1st, 2nd, 3rd 等等。
    + `%Y` 年，4 位数字 
    + `%y` 年，2 位数字
    + `%a` 缩写的星期名字，Sun ~ Sat
    + `%d` 月份中的天数，数字 00 ~ 31
    + `%e` 月份中的天数，数字 0 ~ 31
    + `%m` 月，数字 01 ~ 12
    + `%c` 月，数字 1 ~ 12
    + `%b` 缩写的月份名字 Jan ~ Dec
    + `%j` 一年中的天数 001 ~ 366
    + `%H` 小时 00 ~ 23
    + `%k` 小时 0 ~ 23
    + `%h` 小时 01 ~ 12
    + `%I` 小时 01 ~ 12
    + `%l` 小时 1 ~ 12
    + `%i` 分钟，数字 00 ~ 59
    + `%r` 时间，12 小时 `hh:mm:ss [AP]M`
    + `%T` 时间，24 小时 `hh:mm:ss`
    + `%S` 秒 00 ~ 59
    + `%s` 秒 00 ~ 59
    + `%p` AM 或 PM 
    + `%w` 一个星期中的天数 0=Sunday ~ 6=Saturday
    + `%U` 星期 0 ~ 52，这里星期天是星期的第一天
    + `%u` 星期 0 ~ 52，这里星期一是星期的第一天
    + `%%` 字符 %

    ```sql
    select DATE_FORMAT('1997-10-04 22:23:00','%W %M %Y'); -- 'Saturday October 1997' 
    select DATE_FORMAT('1997-10-04 22:23:00','%H:%i:%s'); -- '22:23:00'     
    select DATE_FORMAT('1997-10-04 22:23:00','%D %y %a %d %m %b %j'); -- '4th 97 Sat 04 10 Oct 277' 
    select DATE_FORMAT('1997-10-04 22:23:00','%H %k %I %r %T %S %w'); -- '22 22 10 10:23:00 PM 22:23:00 00 6' 
    ```

21. `TIME_FORMAT(time,format)` 和 DATE_FORMAT() 类似，但 TIME_FORMAT 只处理小时、分钟和秒，其余符号产生一个`NULL`值或`0`。

22. `CURDATE()` 或 `CURRENT_DATE()` 以`YYYY-MM-DD`或`YYYYMMDD`格式返回当前日期值，根据返回值所处上下文是字符串或数字。
    ```sql
    select CURDATE(); -- '1997-12-15' 
    select CURDATE() 0; --19971215 
    ```

23. `CURTIME()` 或 `CURRENT_TIME()` 以`HH:MM:SS`或`HHMMSS`格式返回当前时间值，根据返回值所处上下文是字符串或数字。
    ```sql
    select CURTIME(); -- '23:50:26' 
    select CURTIME() 0; -- 235026 
    ```

24. `NOW()` 或 `SYSDATE()` 或 `CURRENT_TIMESTAMP()`　以`YYYY-MM-DD HH:MM:SS`或`YYYYMMDDHHMMSS`格式返回当前日期时间，根据返回值所处上下文是字符串或数字。
    ```sql
    select NOW(); -- '1997-12-15 23:50:26' 
    select NOW() 0; -- 19971215235026 
    ```

25. `UNIX_TIMESTAMP()` 或 `UNIX_TIMESTAMP(date)` 返回一个 Unix 时间戳，从`1970-01-01 00:00:00 GMT`开始的秒数，date 默认值为当前时间。
    ```sql
    select UNIX_TIMESTAMP(); -- 882226357 
    select UNIX_TIMESTAMP('1997-10-04 22:23:00'); -- 875996580 
    ```

26. `FROM_UNIXTIME(unix_timestamp)` 以`YYYY-MM-DD HH:MM:SS`或`YYYYMMDDHHMMSS`格式返回时间戳的值，根据返回值所处上下文是字符串或数字。
    ```sql
    select FROM_UNIXTIME(875996580); -- '1997-10-04 22:23:00' 
    select FROM_UNIXTIME(875996580) 0; -- 19971004222300 
    ```

27. `FROM_UNIXTIME(unix_timestamp, format)` 以 format 字符串格式返回时间戳的值。
    ```sql
    select FROM_UNIXTIME(UNIX_TIMESTAMP(),'%Y %D %M %h:%i:%s %x'); -- '1997 23rd December 03:43:30 x' '%Y%m%d%H%i%s'
    ```

28. `SEC_TO_TIME(seconds)` 以`HH:MM:SS`或`HHMMSS`格式返回秒数转成的 TIME 值，根据返回值所处上下文是字符串或数字。
    ```sql
    select SEC_TO_TIME(2378); -- '00:39:38' 
    select SEC_TO_TIME(2378) 0; -- 3938 
    ```

29. `TIME_TO_SEC(time)` 返回 time 值有多少秒。
    ```sql
    select TIME_TO_SEC('22:23:00'); -- 80580 
    select TIME_TO_SEC('00:39:38'); --2378
    ```

---
参考链接

* [MySQL 基础](/note/mysql/brief.md)
* [MySQL 数据类型](/note/mysql/datatype.md)
* [MySQL 中的 SQL 语句](/note/mysql/sql.md)
* [MySQL 索引](/note/mysql/index.md)
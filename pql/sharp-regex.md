# Sharp表达式 - 正则表达式操作

Sharp表达式中关于正则表达式的操作如下：

* `FIND FIRST IN 'str'` 返回正则表达式在字符串的第一个匹配字符串，找不到返回空字符串。
* `FIND FIRST MATCH IN 'str'` 返回正则表达式在字符串的第一个匹配，匹配的分组保存为一个字符串数组，找不到返回一个空数组。
* `FIND ALL IN 'str'` 返回正则表达式在字符串中的所有匹配字符串， 返回一个字符串数组，找不到返回空数组。
    ```sql
    FOR $path IN (FILE LIST 'c:/io.Qross/Folder/pql') LOOP
        SET $content := FILE READ $path;
        FOR $m IN "\(/doc(/[a-z]+)+\)" FIND ALL IN $content LOOP
            SET $content := $content REPLACE $m TO ${ $m REPLACE "/doc" TO "" CONCAT ".md" };
        END LOOP;

        FILE DELETE $path;
        FILE WRITE $path APPEND $content;
    END LOOP;
    ```
    上面是一个替换Markdown文档中链接的例子，找到文件中的所有链接并用FOR语句遍历并挨个替换。
* `FIND ALL MATCH IN 'str'` 返回正则表达式在字符串中的所有匹配，每个匹配分组都存为一个字符串数组，所有会返回一个嵌套数组。可以用FOR语句遍历。
* `REPLACE FIRST IN 'str' TO 'replacement'` 查找正则表达式在字符串中的第一个匹配并替换为新的字符串`replacement`，返回替换后的字符串。
* `REPLACE LAST IN 'str' TO 'replacement'` 查找正则表达式在字符串中的最后一个匹配并替换为新的字符串`replacement`，返回替换后的字符串。
* `REPLACE ALL IN 'str' TO 'replacement'` 查找正则表达式在字符串中的所有匹配并替换为新的字符串`replacement`，返回替换后的字符串。结果同字符串的Link `'str' REPLACE ALL 'regex' TO 'replacement'`。
* `TEST 'str'` 测试字符串是否匹配正则表达式，返回布尔值。与`'str' MATCHES 'regex'`操作一致，只不过互换了参数位置。

---
参考链接

* [更优雅的数据操作方法 Sharp表达式](/pql/sharp.md)
* [Sharp表达式操作 - 数字 INTEGER/DECIMAL](/pql/sharp-numeric.md)
* [Sharp表达式操作 - 字符串 TEXT](/pql/sharp-text.md)
* [Sharp表达式操作 - 日期时间 DATETIME](/pql/sharp-datetime.md)
* [Sharp表达式操作 - 数组 ARRAY](/pql/sharp-array.md)
* [Sharp表达式操作 - 数据行 ROW](/pql/sharp-row.md)
* [Sharp表达式操作 - 数据表 TABLE](/pql/sharp-table.md)
* [Sharp表达式操作 - 数据判断](/pql/sharp-if.md)
* [Sharp表达式操作 - Json字符串](/pql/sharp-json.md)
* [条件表达式](/pql/condition.md)
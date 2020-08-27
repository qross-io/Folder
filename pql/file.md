# 文件操作 FILE语句
FILE语句是一个名词语句，用于文件操作，实现文件新建、删除和列表等操作。
```sql
    FILE "path";
    FILE DELETE "path";
    FILE MOVE "path" TO "new Path";
    FILE RENAME "path" TO "new path";
    FILE COPY "path" TO "new path";
    FILE MAKE "path";
    FILE LENGTH "path";
    FILE SIZE "path";
    FILE LIST "dir";
    FILE WRITE "path" APPEND "content";
    FILE READ "path";
```
FILE语句通过路径`path`来定位文件，必须使用完整的路径。

### 获取文件的详细信息
```sql
VAR $file := FILE 'c:/io.Qross/Home/1.txt';
```
无其他关键词的FILE语句用于获取文件的详细信息，返回值是一个数据行。如果文件不存在，返回格式为：
```json
{
    "path": "c:\\io.Qross\\Home\\1.txt",
    "exists": false
}
```
如果文件存在，返回格式为：
```json
{
    "path": "c:\\io.Qross\\Home\\1.txt",
    "exists": true,
    "name": "1.txt",
    "extension": "txt",
    "last_modified": "2020-08-08 12:02:34",
    "length": 2477,
    "size": "2.41K",
    "parent": "c:\\io.Qross\\Home"
}
```

* `PRINT $file.size;`将打印`2.41K`
* 文件夹的扩展名是`<DIR>`

### 新建和删除文件
```sql
FILE MAKE "c:/io.Qross/Home/1.txt";
FILE DELETE "c:/io.Qross/Home/1.txt";
```
* 新建文件返回布尔值。文件不存在且有写权限可创建成功，否则失败。只能创建文件文件。
* 删除文件操作也返回布尔值。如果文件存在且未被其他程序占用，即删除成功，返回`true`; 删除失败返回`false`。

### 文件重全名、移动和复制。
```sql
FILE RENAME "c:/io.Qross/Home/1.txt" TO "2.txt";
FILE MOVE "c:/io.Qross/Home/1.txt" TO "d:/temp/2.txt";
FILE COPY "c:/io.Qross/Home/1.txt" TO "d:/temp/2.txt";
```
逻辑预计在版本`0.6.5`实现。

### 判断文件是否存在
```sql
IF FILE EXISTS "c:/io.Qross/Home/1.txt" THEN
    PRINT 'yes';
END IF;

IF FILE NOT EXISTS "c:/io.Qross/Home/1.txt" THEN
    PRINT 'no';
END IF;
```
文件是否存在的判断只能用于条件表达式中。

### 文件列表
```sql
FILE LIST "c:/io.Qross/Home/";
```
接受一个文件夹路径，列表此文件下的所有文件（不包括文件夹），返回一个数据表，每个数据行都是一个文件的信息。一般在循环中使用。
```sql
FOR $file OF (FILE LIST "c:/io.Qross/Home/") LOOP
    PRINT $file.name;
END LOOP;
```

### 获取文件大小
```sql
SET $length := FILE LENGTH "c:/io.Qross/Home/1.txt";
SET $size := FILE SIZE "c:/io.Qross/Home/1.txt";
```

* `LENGTH`返回字节长度，是一个整数，如`2477`。
* `SIZE`返回易读的文件大小表示，是一个字符串，如`2.41K`。`SIZE`的逻辑等效于`FILE LENGTH "path" -> TO CAPACITY`。 

### 文件读写操作
```sql
SET $content := FILE READ "c:/io.Qross/Home/1.txt";
SET $content := $content + "\nHELLO WORLD.";
FILE WRITE "d:/temp/2.txt" APPEND $content;
```

* `READ`操作将整个文件内容读成一个字符串，文件不存在得抛出异常。
* `WRITE`向文件中附加文本内容，附加的内容放在文件末尾。如果文件不存在则新建。
* `READ`和`WRITE`适合一次性读取和一性次写入，还有就是读写不规则文件的场景。读取按行分隔的规则文本可以[使用OPEN语句将文件读成数据表](/pql/file-table.md)，写入规则数据可以[用SAVE语句保存为文本文件](/pql/txt.md)。


在PQL中，FILE语句和SQL语句、PARSE语句一样归为查询语句的范畴，每条语句都有返回值，所以SQL语句和PARSE语句的特性FILE语句均支持。比如用做赋值语句、在[FOR循环](/pql/for.md)中使用，使用[Sharp表达式](/pql/sharp.md)再编辑，当做[查询表达式](/pql/query.md)的一部分等。

---
参考链接

* [文件夹操作 DIR](/pql/dir.md)
* [用SQL语句查询文件](/pql/file-table.md)
* [将数据保存为文本文件](/pql/txt.md)
* [将数据保存为CSV文件](/pql/csv.md)
* [更优雅的数据操作方法 Sharp表达式](/pql/sharp.md)
* [嵌入式查询表达式](/pql/query.md)
* [数据类型](/pql/datatype.md)
* [变量声明 SET](/pql/set.md)
* [变量声明 VAR](/pql/var.md)
* [PQL中的SQL语句](/pql/sql.md) 
* [循环遍历 FOR](/pql/for.md)
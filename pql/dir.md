# 文件夹操作 DIR语句
DIR语句和FILE语句一样，也是一个名词语句，主要用于文件的查询或统计。
```sql
    DIR "path";
    DIR LIST "path";
    DIR DELETE "path";
    DIR RENAME "path" TO "new path";
    DIR MOVE "path" TO "new path" REPLACE EXISTING;
    DIR COPY "path" TO "new path" REPLACE EXISTING;
    DIR MAKE "path";
    DIR LENGTH "path";
    DIR SPACE "path";
    DIR SIZE "path";
    DIR CAPACITY "path";
```

### 文件创建和删除
```sql
    DIR MAKE 'd:\temp';
    DIR DELETE 'd:\temp';
```
* `MAKE`返回布尔值，创建成功为`true`，创建失败（如文件夹已存在）为`false`。
* `DELETE`返回布尔值，删除成功为`true`，删除失败（如文件夹中有文件被打开）为`false`。**注意，这条语句会删除文件夹及文件夹下的所有文件！**

### 列表文件和文件夹
```sql
    DIR "d:/temp";
    DIR LIST "d:/temp";
```
* `DIR`返回一个数据表，包含指定文件夹下的所有文件和文件夹，这个命令致敬DOS下的经典命令`dir`。数据表中每个文件的信息格式见[FILE语句](/pql/file.md)。
* `DIR LIST`也返回一个数据表，但是只包含指定文件夹下的子文件夹，不包括文件。

### 文件夹的重命名、移动和复制
```sql
    DIR RENAME "d:\\temp" TO "work";
    DIR MOVE "d:\\temp" TO "d:\\work" REPLACE EXISTSING;
    DIR COPY "d:\\temp" TO "d:\\work" REPLACE EXISTSING;
```

* `RENAME`操作可以为文件夹改名，注意不能指定与源文件夹不同父级目录的路径。返回布尔值。
* `MOVE`操作将整个文件夹及其文件夹下的所有子文件夹移动到目标目录下，源文件夹会删除。`REPLACE EXISTING`选项表示如果目标文件（注意是文件）已存在，则覆盖。返回布尔值。
* `COPY`操作将整个文件夹及其文件夹下的所有子文件夹复制到目标目录下，源文件夹会保留。`REPLACE EXISTING`选项表示如果目标文件（注意是文件）已存在，则覆盖。返回布尔值。


### 获取文件夹的大小或占用空间
```sql
    DIR LENGTH "d:/work";
    DIR SPACE "d:/work";
    DIR SIZE "d:/work";
    DIR CAPACITY "d:/work";
```
* `LENGTH`和`SPACE`功能一致，返回文件夹占用的字节数，如`3539654`。
* `SIZE`和`CAPACITY`功能一致，返回文件夹占用空间的字符串格式信息，如`3.38M`。

### 判断文件夹是否存在
```sql
IF DIR EXISTS "d:/temp" THEN
    PRINT 'yes';
END IF;

IF DIR NOT EXISTS "d:/temp" THEN
    PRINT 'no';
END IF;
```
判断文件夹是否存在只能用在条件表达式中。


DIR语句也和[FILE](/pql/file.md)语句一样，是一个查询语句。可以用做语句赋值、循环遍历、嵌入式查询表达式等各个地方。


---
参考链接

* [文件操作 FILE](/pql/file.md)
* [更优雅的数据操作方法 Sharp表达式](/pql/sharp.md)
* [嵌入式查询表达式](/pql/query.md)
* [数据类型](/pql/datatype.md)
* [变量声明 SET](/pql/set.md)
* [变量声明 VAR](/pql/var.md)
* [PQL中的SQL语句](/pql/sql.md) 
* [循环遍历 FOR](/pql/for.md)
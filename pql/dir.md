# 文件夹操作 DIR语句
DIR语句和FILE语句一样，也是一个名词语句，主要用于文件的查询或统计。
```
    DIR "path";
    DIR LIST "path";
    DIR DELETE "path";
    DIR MOVE "path" TO "new path";
    DIR RENAME "path" TO "new path";
    DIR COPY "path" TO "new path";
    DIR MAKE "path";
    DIR LENGTH "path";
    DIR SPACE "path";
    DIR SIZE "path";
    DIR CAPACITY "path";
```

#### 文件创建和删除
```
    DIR MAKE 'd:\temp';
    DIR DELETE 'd:\temp';
```
* `MAKE`返回布尔值，创建成功为`true`，创建失败（如文件夹已存在）为`false`。
* `DELETE`返回布尔值，删除成功为`true`，删除失败（如文件夹中有文件被打开）为`false`。

#### 列表文件和文件夹
```
    DIR "d:/temp";
    DIR LIST "d:/temp";
```
* `DIR`返回一个数据表，包含指定文件夹下的所有文件和文件夹，这个命令致敬DOS下的经典命令`dir`。数据表中每个文件的信息格式见[FILE语句](/doc/pql/file)。
* `DIR LIST`也返回一个数据表，但是只包含指定文件夹下的子文件夹，不包括文件。

#### 文件夹的重命名、移动和复制
```
    DIR RENAME "d:\\temp" TO "d:\\work";
    DIR MOVE "d:\\temp" TO "d:\\work";
    DIR COPY "d:\\temp" TO "d:\\work";
```
文件夹的重命名、移动和复制逻辑预计在版本`0.6.5`实现。

#### 获取文件夹的大小或占用空间
```
    DIR LENGTH "d:/work";
    DIR SPACE "d:/work";
    DIR SIZE "d:/work";
    DIR CAPACITY "d:/work";
```
* `LENGTH`和`SPACE`功能一致，返回文件夹占用的字节数，如`3539654`。
* `SIZE`和`CAPACITY`功能一致，返回文件夹占用空间的字符串格式信息，如`3.38M`。

#### 判断文件夹是否存在
```
IF DIR EXISTS "d:/temp" THEN
    PRINT 'yes';
END IF;

IF DIR NOT EXISTS "d:/temp" THEN
    PRINT 'no';
END IF;
```
判断文件夹是否存在只能用在条件表达式中。


DIR语句也和[FILE](/doc/pql/file)语句一样，是一个查询语句。可以用做语句赋值、循环遍历、嵌入式查询表达式等各个地方。


---
参考链接
* [文件操作 FILE](/doc/pql/file)
* [更优雅的数据操作方法 Sharp表达式](/doc/pql/sharp)
* [嵌入式查询表达式](/doc/pql/query)
* [数据类型](/doc/pql/datatype)
* [变量声明 SET](/doc/pql/set)
* [变量声明 VAR](/doc/pql/var)
* [PQL中的SQL语句](/doc/pql/sql) 
* [循环遍历 FOR](/doc/pql/for)
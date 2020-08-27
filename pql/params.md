# 向PQL过程传递参数
任何程序都可以接受外部参数以确定执行不同的操作，PQL过程一样也可以接受外部传递的参数。

* 在[Keeper调度工具]中执行PQL命令时：
  ```s
  PQL /usr/qross/pql/test.sql --args name=Tom&age=18&score=89
  ```
* 手工执行PQL文件时
  命令可以在本地开发环境和服务器上运行。
  ```s
  java -jar qross-worker-0.6.3.jar --file /usr/qross/pql/test.sql --args name=Tom&age=18&score=89
  ```
* OneApi传递参数
  OneApi使用Http机制传递参数，比如URL地址或Json参数。
  ```
  http://yourdomain.com/api/test?name=Tom&age=18&score=89
  ```
* Voyager模板引擎中传递参数
  模板引擎也使用Http传递参数，不过接收浏览器地址栏中输入的参数。
  ```
  http://yourdoman/page/test?name=Tom&age=18&score=89
  ```
* 使用PQL类运行PQL时
  PQL类提供了传递参数的多个重载方法，见[PQL类](/pql/class.md)。

### PQL如何接收和处理参数
1. 第一种方式是在PQL设置占位符，如`#{name}`，见下例：
  ```sql
  UPDATE students SET score=#{score} WHERE name='#{name}';
  ```
  `#{`和`}`包围的内容仅仅是一个占位，会把传递过来的数据原封不动的替换占位符。上例在替换占位符之后就变成了：
  ```sql
  UPDATE students SET score=89 WHERE name='Tom';
  ```
  特别的，因为不对数据做处理，也可以出现SQL注入问题，比如上面name参数中带有单引号，会让UPDATE语句出现执行错误。PQL已经提供了解决方法：
  ```sql
  UPDATE students SET score=#{score} WHERE name='#{'name'}';
  ```
  就是在参数名两端再加一对单引号`#{'name'}`，对于双此号场景，同样也支持。
  ```sql
  UPDATE students SET score=#{score} WHERE name="#{"name"}";
  ```
  如果包围参数占位符的是单引号，处理SQL注入就用单引号; 如果包围占位符的是双引号，就用双引号处理SQL注入。

  因为参数占位符不处理数据类型，并且在PQL其 **执行优先级最高**，所以我们可以用参数占位符干很多事。
  ```sql
  SELECT * FROM movie_#{age} WHERE movie_name='#{name} and Jerry'; 
  ```
  替换成参数值就变成了
  ```sql
  SELECT * FROM movie_18 WHERE movie_name='Tom and Jerry';
  ```
  参数占位符必须由`#{`和`}`包围，大括号不可省略。占位符可以设置在PQL任意的位置，甚至是语句关键词。  
  需要注意的是，未传递变量时参数占位符不会被替换，仍保持原状态`#{name}`，很可能会导致程序运行出错或结果不正确，请一定认真检查。可以通过`IF '#{name}' IS NOT UNDEFINED THEN`判断是否传参。
2. 第一种方式是使用参数变量，如`$name`：
  ```sql
  UPDATE students SET score=$score! WHERE name=$name;
  ```
  PQL会把所有传递的参数自动生成用户变量，但是这些变量类型都是字符串类型，如果是数字使用时需要自已转换。如上例`$score!`，叹号`!`表示忽略引号。
  用户变量不需要考虑SQL注入的问题，可以根据自己的喜好选用哪种方式处理参数。  
  未传递的参数不会被自动生成用户变量，使用时仍然是`UNDEFINED`状态，请一定注意。可以通过`IF $name IS NOT UNDEFINED THEN`进行判断是否传递了参数。

---
参考链接

* [io.qross.pql.PQL 类](/pql/class.md)
* [PQL中的变量](/pql/variable.md)
* [任务调度工具 Keeper](/keeper/overview.md)
* [PQL执行器 Worker](/worker/overview.md)
* [统一接口 OneApi](/oneapi/overview.md)
* [模板引擎 Voyager](/voyager/overview.md)
# 发送邮件（报表） SEND语句
在数据开发中，经常有发送邮件报表的需求。PQL提供功能完整的发件邮件功能。

### 基本语句
```sql
SEND EMAIL "test mail"
    SET CONTENT "hello world"
    TO "personal<user@domain.com>"
    CC "user2@domain.com; personal3<user3@domain.com>"
    BCC "user4@domain.com";
```
* 使用`SEND MAIL`或`SEND EMAIL`语句发送邮件。
* `MAIL`关键词后面是邮件标题。
* `SET CONTENT`用来设置文件内容，`SET`关键词可省略。一般使用邮件模板而不是直接设置内容，数据报表一般内容比较多。
* `TO`关键词用来设置接收人，可以直接写邮件地址，也可以使用上例中“署名<邮件地址>”的格式。多个收件人之间使用逗号`“,”`或分号`“;”`隔开。
* `CC`关键词设置抄送人。`BCC`设置暗送人。
* 发送邮件至少设置一个收件人。
* 发件人设置见文后说明。

### 使用模板和签名
```sql
SEND MAIL "test mail"
    USE TEMPLATE "template.html"
    WITH SIGNATURE "signature.html"
    TO "personal<user@domain.com>";

SEND MAIL "test mail"
    USE DEFAULT TEMPLATE
    WITH DEFAULT SIGNATURE
    TO "personal<user@domain.com>";
```
* 使用`USE TEMPLATE`指定一个HTML模板，`USE`关键词可省略。`USE DEFAULT TEMPLATE`表示使用默认邮件模板，默认模板需要预先保存在Qross系统的Email模板目录`%QROSS_HOME/templates/email/`下，文件名必须为`default.html`。
* 邮件模板`USE TEMPLATE`和邮件内容`SET CONTENT`二选一，邮件模板优先级更高。
* 使用`WITH SIGNATURE`设置一个邮件签名，签名文件也可以放在`resources`目录下。签名文件的内容会替换模板中的占位符`#{signature}`; 如果模板中未设置占位符，则签名内容自动附在`</body>`标签之前; 如果邮件模板没有`</body>`标签，则签名内容附在整个模板文件之后。也可使用`WITH DEFAULT SIGNATURE`设置默认签名，默认签名需要预先保存在Qross系统的Email模板目录`%QROSS_HOME/templates/email/`下，文件名必须为`signature.html`。
* 关于邮件模板和签名的保存位置：模板可以保存在磁盘中，使用完整路径引用；也可以放在项目的`resources`目录下，使用相对路径引用；还可以放在Qross系统家目录下，位置`%QROSS_HOME/templates/email/`，使用文件名引用。

### 邮件内容编辑
无论是使用邮件模板还是手工设置邮件内容，里面的内容都不能是固定的，需要根据计算结果或应用场景生成不同的内容。有几种方式生成或更新邮件内容。
* 在邮件模板或内容中使用嵌入式PQL代码，可以直接将查询语句写在邮件模板中。详见[Voyager模板引擎](/voyager/overview.md)。
  ```html
  <div>高三年级期中考试成绩单 <%=@NOW FORMAT 'yyyy年M月d日'%></div>
  <div>
    <%=SELECT id, name, score FROM students -> TO HTML TABLE %>
  </div>
  ```
  上例中`<%`和`%>`包围的部分就是嵌入式PQL的代码，简单直接。
* 第二种方式是使用PQL的参数占位符，格式为`#{data}`。通过`PLACE DATA`替换邮件内容中的占位符，其中`PLACE`可省略。PQL代码为：
  ```sql
    SET $name := 'Tom';
    SET $score := 89;    
    SEND MAIL "test mail"
        USE DEFAULT TEMPLATE
        PLACE DATA """date=${@NOW FORMAT 'yyyy年M月d日'}&name=$name&score=$score"""
        TO "personal<user@domain.com>";
  ```
  邮件模板代码为：
  ```html
  <div>高三年级期中考试成绩单 #{date}</div>
  <div>
    <div>姓名：#{name}</div>
    <div>分数：#{score}</div>
    <div>
  </div>
  ```
  最后解析完的邮件内容为：
  ```html
  <div>高三年级期中考试成绩单 2020年4月15日</div>
  <div>
    <div>姓名：Tom</div>
    <div>分数：89</div>
    <div>
  </div>
  ```
  `PLACE DATA`可使用多次。
* 第三种方式是使用`PLACE`和`AT`组合，用数据替换自定义的占位符。
  ```sql
    SEND MAIL "test mail"
        USE DEFAULT TEMPLATE
        PLACE ${ @NOW FORMAT 'yyyy年M月d日' } AT '#{date}'
        PLACE $name AT '#{name}'
        PLACE $score AT '#{score}'
        TO "personal<user@domain.com>";
  ```
  `AT`后面的占位符格式可自定义。

### 添加附件
```sql
SEND EMAIL "test mail"
    SET CONTENT "hello world"
    ATTCH "scores.xlsx"
    TO "personal<user@domain.com>";
```
* 如果只指定文件名，默认查找临时目录下（`%QROSS_HOME/temp/`）的文件，也可以指定完整路径。这里有一个技巧，就是通过[SAVE语句](/pql/excel.md)生成的文件，可直接`ATTACH`到邮件附件中，两边都不需要指定完整目录，只设置文件名即可。
* `ATTACH`可以使用多次以附加多个文件。

### 邮件发送设置
一般情况下，发件人信息只需要在系统环境里进行一次性设置，见[系统设置](/pql/setup.md)。但如果未进行系统设置或临时改变发件人邮件，可以在SEND语句中进行设置。
```sql
SEND EMAIL "test mail"
    SET SMTP HOST "smtp.domain.com"
    SET PORT 25
    FROM "personal<sender@domain.com>"
    SET PASSWORD "password"
    SET PERSONAL "name"
    SET LANGUAGE "Chinese"
    TO "personal<user@domain.com>";
```
* `SET SMTP HOST` 设置SMTP服务器域名或IP，`SET`关键词可省略。
* `SET PORT` 设置SMTP服务器端口，默认`25`，SSL下默认`465`，`SET`关键词可省略。
* `FROM` 设置发件人邮箱地址。
* `SET PASSWORD` 设置发件人邮箱密码，`SET`关键词可省略。
* `SET PERSONAL` 设置发件人署名，`SET`关键词可省略。也可用来修改系统默认设置的署名。
* `SET LANGUAGE` 设置模板中内容的交互语言，邮箱模板已支持多语言，需要自定义。详见[Voyager模板引擎国际化](/voyager/language.md)

---
参考链接
* [PQL系统设置](/pql/setup.md)
* [嵌入式PQL](/pql/embedded.md)
* [Voyager模板引擎](/voyager/overview.md)
* [Voyager国际化](/pql/language.md)
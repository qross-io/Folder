# Marker 类 - Markdown 转 Html

Marker 可以将 Markdown 格式的文档转成 Html 代码，以方便在网页中显示，官网和 Master 管理平台中的文档全是使用 Marker 进行格式转换的。并且 Marker 也是 Voyager 模板引擎的 Markdown 格式转换引擎。

Marker 是一个 Java 类，如果你已经[引入了 PQL 依赖](/pql/use-pql.md)，可以通过路径`io.qross.app.Marker`找到。以下是 Marker 公开的方法

```java
public Marker(String content); //构造函数

public static String markdownToHtml(String content); //将 markdown 字符串转成 HTML，一般只需要使用这一个方法足够
public static Marker openFile(String path); //打开本地或资源目录下的 markdown 文件
public static Marker open(String content); //功能同构造函数 

public Marker removeHeader(); //移除大标题
public Marker replaceInMarkdown(String old, String replacement); //在转换前替换
public Marker replaceAllInMarkdown(String regex, String replacement);　//在转换前用正则表达式替换
public Marker replaceInHtml(String old, String replacement);　//在转换后替换
public Marker replaceAllInHtml(String regex, String replacement);　//在转换后使用正则表达式替换

public Marker colorCodes(); //使用彩色编码，显示行号，需要 root.js 组件库支持
public Marker colorCodes(boolean lineNumbers); //使用彩色编码，是否显示行号
public Marker transform(); //执行转换
public String getTitle(); //获取文档大标题
public String getContent(); //获取全部内容
public String getSummary(); //获取文章第一自然段作为摘要。
```

调用示例：

```java
public static String loadContent(String project, String page) {
    return Marker.openFile("/project/page.md")
            .replaceInMarkdown(".md)", ")")
            .transform()
            .colorCodes(false)
            .getContent();
}

Marker.markdownToHtml(content);
```

---
参考链接

* [Voyager 模板引擎](/voyager/overview.md)
* [Voyager 中的 Markdown 文档格式](/voyager/markdown.md)
* [颜色编码表](/root.js/colors.md)
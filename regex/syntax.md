
<style type="text/css">
table[note] tr td:first-child { text-align: center; font-family: consolas; width: 100px; }
</style>

# 正则表达式语法

正则表达式是查找匹配字符中的强大手段，正则表达式对于处理字符串的重要性甚至可以理解为 SQL 语句对操作数据库的重要性。而且正则表达式是行业标准，适用于任何语言。

## 常用字符

-- 20 --

<table width="100%" border="0" cellspacing="1" note>
    <tr>
        <td>.</td>
        <td>匹配任意单个字符，默认不包括回车符<code>\r</code>和换行符<code>\n</code>，除非使用了修饰符<code>s</code></td>
    </tr>
    <tr>
        <td>[\s\S]</td>
        <td>匹配任意单个字符，包括回车符和换行符</td>
    </tr>
    <tr>
        <td>\d 或 [0-9]</td>
        <td>匹配任意数字</td>
    </tr>
    <tr>
        <td>\D</td>
        <td>匹配任意一个不是数字的字符，与<code>\d</code>相反，也可以写作<code>[^\d]</code>或<code>[^0-9]</code></td>
    </tr>
    <tr>
        <td>[a-z]</td>
        <td>匹配任意小写英文字符</td>
    </tr>
    <tr>
        <td>[A-Z]</td>
        <td>匹配任意大写英文字符</td>
    </tr>
    <tr>
        <td>\w</td>
        <td>匹配任意阿拉伯字母，数字和下划线, 相当于<code>[0-9a-zA-Z_]</code></td>
    </tr>
    <tr>
        <td>\W</td>
        <td>与<code>\w</code>相反，<br/>相当于<code>[^0-9a-zA-Z_]</code></td>
    </tr>
    <tr>
        <td>\s</td>
        <td>匹配空白字符，包括空格符，制表符，换行符，换页符和其他空格字符</td>
    </tr>
    <tr>
        <td>\S</td>
        <td>匹配一个非空白符，与<code>\s</code>相反</td>
    </tr>
</table>

## 字符集合

使用方括号`[`和`]`包围起来表示字符集合，在字符集合中特殊字符可以不使用反斜杠`\`进行转义，除了`[`，`]`和`-`。

<table width="100%" border="0" cellspacing="1" note>
    <tr>
        <td>[abc]</td>
        <td>字符组，表示匹配集合中的任意一个字符。</td>
    </tr>
    <tr>
        <td>-</td>
        <td>表示区间，如`[a-z]`表示匹配所有英文字母，`[0-9]`表示匹配所有数字<br/>
            如果要在字符集合中匹配中横线，可以使用`\`进行转义，也可以放在字符集合的最后，如`[a-z0-9_-]`</td>
    </tr>
    <tr>
        <td>[^abc]</td>
        <td>反义字符组，匹配不是集合中字符的一个字符，可以用`-`来指定范围。例如`[^\d]`表示所有不含数字</td>
    </tr>
    <tr>
        <td>[a-z^ou]</td>
        <td>混合用法，表示匹配字母<code>a-z</code>但不包括字母`o`和`u`</td>
    </tr>
</table>

## 重复

-- 20 --

<table width="100%" border="0" cellspacing="1" note>
    <tr>
        <td>?</td>
        <td>匹配 0 次或 1 次，表示有或者没有</td>
    </tr>
    <tr>
        <td>+</td>
        <td>重复一次或多次，即至少一次</td>
    </tr>
    <tr>
        <td>*</td>
        <td>重复 0 次或多次</td>
    </tr>
    <tr>
        <td>{n}</td>
        <td>重复 n 次</td>
    </tr>
    <tr>
        <td>{m,n}</td>
        <td>重复最少 m 次，最多 n 次</td>
    </tr>
    <tr>
        <td>{m,}</td>
        <td>重复最少 m 次</td>
    </tr>
</table>


## 匹配组或分组

-- 20 --

<table width="100%" border="0" cellspacing="1" note>
    <tr>
        <td>( )</td>
        <td>匹配组在匹配到结果时可以直接通过索引得到相应的捕获结果</td>
    </tr>
    <tr>
        <td>|</td>
        <td>表示“或”，如<code>(hello|hi)</code>匹配<code>hello</code>>或<code>hi</code>都可以</td>
    </tr>
</table>

## 边界

-- 20 --

<table width="100%" border="0" cellspacing="1" note>
    <tr>
        <td>^</td>
        <td>不在<code>[ ]</code>中时，匹配字符串每行的开始</td>
    </tr>
    <tr>
        <td>$</td>
        <td>匹配字符串每行的结束</td>
    </tr>
    <tr>
        <td>\b</td>
        <td>匹配零宽单词边界，如<code>/[a-z]+\b/</code>匹配字符串<code>"Hello World"</code>，得到<code>Hello</code></td>
    </tr>
    <tr>
        <td>\B</td>
        <td>匹配非零宽单词边界，如<code>/[a-z]+\B</code>匹配字符串<code>`"Hello World"</code>，得到<code>Hell</code></td>
    </tr>
</table>

## 非贪婪模式

-- 20 --
            
<table width="100%" border="0" cellspacing="1" note>
    <tr>
        <td>?</td>
        <td>在重复符号<code>+</code>后面加上问号<code>?</code>表示非贪婪匹配，如<code>\d+?</code>，匹配结果返回尽可能少的字符，否则返回尽可能多的字符</td>
    </tr>
</table>

## 空白字符

-- 20 --

<table width="100%" border="0" cellspacing="1" note>
    <tr>
        <td>\s</td>
        <td>匹配任意空白字符</td>
    </tr>
    <tr>
        <td>&nbsp;</td>
        <td>匹配空格</td>
    </tr>
    <tr>
        <td>\t</td>
        <td>匹配一个水平制表符</td>
    </tr>
    <tr>
        <td>\r</td>
        <td>匹配一个回车符</td>
    </tr>
    <tr>
        <td>\n</td>
        <td>匹配一个换行符</td>
    </tr>
    <tr>
        <td>\v</td>
        <td>匹配一个垂直制表符</td>
    </tr>
</table>

## 特殊字符

-- 20 --

<table width="100%" border="0" cellspacing="1" note>
    <tr>
        <td>\</td>
        <td>转义符号</td>
    </tr>
    <tr>
        <td>\.</td>
        <td>匹配符号 <code>.</code></td>
    </tr>
    <tr>
        <td>\*</td>
        <td>匹配符号 <code>*</code></td>
    </tr>
    <tr>
        <td>\+</td>
        <td>匹配符号 <code>+</code></td>
    </tr>
    <tr>
        <td>\?</td>
        <td>匹配符号 <code>?</code></td>
    </tr>
    <tr>
        <td>\^</td>
        <td>匹配符号 <code>^</code></td>
    </tr>
    <tr>
        <td>\$</td>
        <td>匹配符号 <code>$</code></td>
    </tr>
    <tr>
        <td>\|</td>
        <td>匹配符号 <code>|</code></td>
    </tr>
    <tr>
        <td>\( 和 \)</td>
        <td>匹配符号 <code>(</code> 和 <code>)</code></td>
    </tr>
    <tr>
        <td>\[ 和 \]</td>
        <td>匹配符号 <code>[</code> 和 <code>]</code></td>
    </tr>
    <tr>
        <td>\{ 和 \}</td>
        <td>匹配符号 <code>{</code> 和 <code>}</code></td>
    </tr>
    <tr>
        <td>\\</td>
        <td>匹配符号 <code>\</code></td>
    </tr>
</table>


## 断言

断言用于查找匹配的前面的前面或后面是或者不是指定的字符或匹配，非常有用。“零宽”表示指定的匹配不返回；“正向”表“肯定”、“是”，“负向”表“否定”、“否”；“先行”指从左向右查找匹配，也可以理解是指定要匹配结果右侧的字符，“后行”是要匹配结果左侧是或者不是指定的字符。

<table width="100%" border="0" cellspacing="1" note>
    <tr>
        <td>(?=pattern)</td>
        <td>零宽正向先行断言，设定指定匹配后面有哪些字符</td>
    </tr>
    <tr>
        <td>(?!pattern)</td>
        <td>零宽负向先行断言，设定指定匹配后面没有哪些字符</td>
    </tr>
    <tr>
        <td>(?&lt;=pattern)</td>
        <td>零宽正向后行断言，设定指定匹配前面有哪些字符</td>
    </tr>
    <tr>
        <td>(?&lt;!pattern)</td>
        <td>零宽负向后行断言，设定指定匹配前面没有哪些字符</td>
    </tr>
    <tr>
        <td>(?:pattern)</td>
        <td>非捕获组，表示匹配但不在结果集中返回</td>
    </tr>
</table>

---
参考链接

* [Javascript 相关操作](/regex/javascript.md)
* [Java 相关操作](/regex/java.md)
* [Scala 相关操作](/regex/scala.md)
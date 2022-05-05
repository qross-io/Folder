# Javascript 正则表达式操作示例

关于 Javascript 中正则表达式对象的基础知识见 [Javascript 中的正则表达式](/regex/javascript.md)。

## 声明一个正则表达式对象

不带修饰符的声明

```javascript
let r = /\d+/;
let r = new RegExp('\\d');
```

带修饰符

```javascript
let r = /[a-z]/i; //忽略大小写
let r = new RegExp('[a-z]', 'i');
```

带两个修改符

```javascript
let r = /[a-z]/ig; //忽略大小写+全局匹配
let r = new RegExp('[a-z]', 'ig');
```

## 改变修饰符

正则表达式对象的修饰符属性是只读的，不能赋值。

```javascript
let r = /[a-z]+/g;
r.ignoreCase = true; //不能这么写
```

如果想为正则表达式添加修饰符，只能重新声明一下。

```javascript
let r = /[a-z]+/g;
r = new RegExp(r, 'i'); //结果是 /[a-z]+/i
```

重新声明时会覆盖修改符，而不是添加。所有正确的做法是。

```javascript
let r = /[a-z]+/g;
r = new RegExp(r, r.flags + 'i'); //结果是 /[a-z]+/gi
```

不能为一个正则表达式添加多个重复的修饰符，否则会报错。

```javascript
/[a-z]+/gig; //错误
new RegExp('[a-z]+', 'iig'); //还是错误
```

## 测试是否与字符串匹配

这里建议使用`test`方法，返回一个布尔值。

```javascript
if (/\d+/.test('123')) {
    //...
}
```

也可以使用`exec`方法。

```javascript
if (/\d+/.exec('123') != null) {
    //...
}
```

或者字符串的`match`方法。

```javascript
if ('123'.match(/\d+/) != null) {
    //...
}
```

其他如`matchAll`方法也可以用，不过没有必要。

## 查找一个匹配

可以使用`exec`方法。

```javascript
let m = /\d/.exec('123') //得到第一个匹配
```

其中`m`是一个数组对象，上例结果是`['1', index: 0, input: '123', groups: undefined]`。其中`m[0]`即匹配结果，`index`属性是匹配结果在字符串中的索引位置，`input`是原始字符串，`groups`是已命名的捕获组，后面讲。

也可以使用字符串的`match`方法。

```javascript
let m = '123'.match(/\d/); //结果与 exec 方法相同
```

## 查找所有匹配

`exec`方法除了查找第一个匹配以外，还可以查找所有匹配，但要求正则表达式必须有`g`修饰符。

```javascript
let r = /\d/g;
let s = '123';
r.exec(s) //得到第一个匹配，结果是 ['1', index: 0, input: '123', groups: undefined]
r.exec(s) //得到第二个匹配，结果是 ['2', index: 1, input: '123', groups: undefined]
r.exec(s) //得到第三个匹配，结果是 ['3', index: 2, input: '123', groups: undefined]
r.exec(s) //没有匹配，结果是 null
```

那么完整的程序就是

```javascript
let r = /\d/g;
let s = '123';
let m = null;
let t = []; //所有匹配结果
do {
    m = r.exec(s);
    if (m != null) {
        t.push(m);
    }
}
while(m != null);
```

如果只关心匹配结果字符串，而不需要索引位置和分组信息，可以使用字符串的`match`方法，也要求必须有`g`修饰符，否则只能得到第一个匹配。

```javascript
'123'.match(/\d/g) //结果是 ['1', '2', '3']
```

简单太多了吧，如果需要索引位置和分组信息又想简单怎么办？ES 11 新增了`matchAll`方法，也要求必须有`g`修改符，否则会报错。

```javascript
'123'.matchAll(/\d/g)
```

方法`matchAll`的执行结果是一个`RegExpStringIterator`迭代器对象，可以转成数组方便遍历。

```javascript
[...'123'.matchAll(/\d/g)]
```

## 替换字符串内容

用的最多的方法是`replace`，例如：

```javascript
'abab'.replace('a', 'b'); //结果是 'bbab'
```

但是这个方法第一个参数如果是字符串的话，则只替换一次。可以使用 ES 12 新增的`replaceAll`方法。

```javascript
'abab'.replaceAll('a', 'b'); //结果是 'bbbb'
```

第一个参数可以是正则表达式，例如：

```javascript
'abcb'.replace(/[ac]/, 'b'); //结果是 'bbcb'
```

如果想全部替换的话，要加上修饰符`g`。

```javascript
'abcb'.replace(/[ac]/g, 'b'); //结果是 'bbbb'
```

如果使用`replaceAll`方法，也要添加修饰符`g`，否则会报错。

```javascript
'abcb'.replaceAll(/[ac]/g, 'b'); //结果是 'bbbb'
'abcb'.replaceAll(/[ac]/, 'b'); //报错
```

## 反向引用和箭头函数

Javascript 与其他语言不同，有反向引用功能，在使用`repalce`方法时，要方便太多。在 Javascript 中，可以使用`$n`表示匹配到的分组捕获内容，支持`$0`到`$9`。先看一下示例。

```javascript
'04/15/2022'.replace(/(\d{2})\/(\d{2})\/(\d{4})/, '$3-$1-$2') //结果是 2022-04-15
```

上例`replace`方法中第一个参数的正则表达式设置了三个捕获组`\d{2}`、`\d{2}`、`\d{4}`，结果分别对应`$1`、`$2`和`$3`（`$0`是整个匹配结果字符串），然后在第二个参数中引用，Javascript 会自动将三个占位符替换为相应的分组结果字符串。

通过`$n`可以对匹配分组结果进行直接引用，但是反向引用不能对结果进行再加工，这时就要用到函数。

```javascript
`border-bottom-width`.replace(/-([a-z])/g, (m, n) => n.toUpperCase()) //结果是 borderBottomWidth
```

正则表达式`-([a-z])`中有一个分组，所以每个匹配结果有两项，上例中的两个匹配结果分别为`["-b", "b"]`和`["-w", "w"]`。所以函数`(m, n) => n.toUpperCase()`中设置了两个参数`m`和`n`，分别对应分组结果`0`和`1`，即`m`对应`-b`和`-w`，`n`对应`b`和`w`。函数中未对`m`进行加工，只对`n`进行了大写转换。

```javascript
'borderBottomWidth'.replace(/[A-Z]/g, u => '-' + u.toLowerCase()) //结果是 `border-bottom-width`
```

上例正则表达式匹配结果没有分组，所有可以在函数只声明一个参数`u`。

复杂的逻辑可以使用多行处理逻辑。

```javascript
'borderBottomWidth'.replace(/[A-Z]/g, u => {
    //more code...
    return '-' + u.toLowerCase();
})
```

## 拆分字符串

字符串的`split`方法也支持传入正则表达式。

```javascript
'a,b,c'.split(','); //结果是 ['a', 'b', 'c']
'a1b2c3'.split(/\d/); //结果也是 ['a', 'b', 'c']
'a12'.split(/\d/); //结果是 ['a', '', '']
```

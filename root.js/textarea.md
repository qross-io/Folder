# TEXTARA 多行输入框扩展

多行输入框在实际开发中使用不多，暂时也没有扩展太多内容。

## 扩展属性

* `readonly` 设置和获取文本框是否只读，重写了原属性。
* `disabled` 设置和获取文本框是否禁用，重写了原属性。
* `enabled` 设置和获取文本框是否启用，新增属性，优先级低于`disabled`。
* `selection` 获取或设置文本框中选择的文本。获取时，如果没有文本被选择，则返回空；设置时，如果指定字符串不在文本框中时，则无效。

## 扩展方法

* `find(strOrRegex)` 在多行输入框中查找指定的字符串，或按正则表达式查找，并选中匹配的文本，多次调用或不断向下查找，方法返回输入框本身。
* `findAll(strOrRegex` 在多行输入框中查找指定的字符串，或按正则表达式查找，返回所有匹配的数组，并选中第一个匹配的文本。数组项包括三个参数`group`、`start`、`end`，`group`是正则表达式的分组结果数组，`start`是匹配结果字符串在原始文件中的开始位置，`end`是匹配结果字符串的结束位置。如下例：
    ```json
    [
        {
            'group': ['hello world', 'hello', 'world'],
            'start': 0,
            'end': 10
        }
    ]
    ```
* `scrollToSelection()` 在因为输入框中的文字过多而出现滚动条时，如果选中的文字不可见，这个方法可以让滚动条自动滚动到选择文字区域。在自动选中文本时才有些用。
* `selectRange(start, end)` 选择指定索引范围的文字。
* `setCursorPosition(position)` 设置文本框的光标所在位置，`position`需要大于`0`，如果`position`大于等于文本框的文本长度，则光标在末尾。
* `replace(str, replacement)` 替换输入框中第一个的字符串`str`为`replacement`。
* `replaceAll(str, replacement)` 替换输入框中所有字符串`str`为`replacement`。

## 扩展事件

扩展事件仅有一个`onmatch`，每一次使用`find`方法时触发，事件参数中可以获得是否找到匹配的字符串。如下例：

```javascript
$('#TextArea1').onmatch = function(e) {
    if (e.detail.matched) {
        //...
    }
}
```

---
参考链接

* [root.js 基本库](/root.js/root.md)
* [INPUT 标签扩展](/root.js/input.md)
* [BUTTON 标签扩展](/root.js/button.md)
# DIV 布局增强

基于 DIV 进行扩展，以方便进行各种页面布局。通过扩展属性`display`来设置不同的模式。所有布局模式只支持页面第一次加载时应用。

```html
<div display="...">...</div>
```

## Panel 模式

Panel 模式用于流式布局，一般用于多项内容的列表显示，根据屏幕宽度显示不同数量的 Panel 并自动换行。DIV 的`display`属性设置为`panel`。

```html
<div display="panel" width="330" height="100">...</div>
```

* `width` 用于设置面板宽度，默认值`330px`。
* `height` 用于设置面板的高度，无默认值。

## Justify 模式

Justify 模式可以平均分布 DIV 内的各个元素，各元素横向分布且间距相等。 

```html
<div display="justify">
    <div class="score">...</div>
    <div class="score">...</div>
    <div class="score">...</div>
    <div class="score">...</div>
    <div class="score">...</div>
</div>
```

## Column 模式

Column 模式可以让 DIV 内的子 DIV 元素进行分栏，并且子 DIV 的宽度可以通过`width`属性进行设置，支持表达式运算。

```html
<div display="column">
    <div width="45%">...</div>
    <div width="55% - 10px">...</div>
</div>
```

---
参考链接

* [root.js 基本库](/root.js/root.md)
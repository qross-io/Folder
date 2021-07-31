
# BackTop 返回顶部

Backtop 组件会在页面右下角增加一个回到顶部的跳转链接，在页面时非常长时会很有用。

需要引用的文件：

```html
<script type="text/javascript" src="/root.js">
<script type="text/javascript" src="/root.backtop.js">
```

HTML 标签样式：

```html
<backtop name="BackTop1" anchor="TOP" opacity="0">Back Top</backtop>
```

这个标签会自动生成一个浮动的 DIV 元素，里面有一个 A 标签。其中：

* `name` 标签的名字或 id，一般不需要设置。
* `anchor` 为 A 标签的链接地址，即链接指向的锚点，一般情况下无需设置。组件或自动使用 BODY 中第一个元素作为锚点。如果第一个元素的样式属性`position`的值如果为`fixed`，则需要设置这个值为其他元素。
* `opacity` DIV 初始透明度，默认值为`0`。当页面上首屏时，一般 DIV 会隐藏显式，当页面向下滚动时，DIV 会逐渐显现出来。
* `Back Top` 为 A 标签的文本，默认值为`TOP`。

所以，BACKTOP 标签也不是必须的，只要引用了`root.backtop.js`，页面会自动添加上面的 DIV 元素。

还自定义 DIV 中的内容：

```html
<div backtop>
    <div>其他内容......</div>
    <div><button href="#Top">回到页面顶部</button></div>    
</div>
```

这种情况下需要手动设置跳转锚点，并且手动添加返回链接，上例是`#Top`。组件只是将这个 DIV 固定到页面右下角，并在页面向下滚动时显示出来。上例的`backtop`属性不可省略。当前页面使用了自定义的“返回顶部”内容，除了返回页面顶部以外，还可以在页面内容之间进行导航。

BACKTOP 没有方法，但有一个事件`onback`，使用事件时必须声明`name`属性：

```html
<backtop onback="alert(1)"></backtop>
```

或使用事件绑定：

 ```javascript
$listen('#name').on('back', function() {
    alert(1);
});
 ```


---
参考链接

* [root.js 基础库](/root.js/root.md)
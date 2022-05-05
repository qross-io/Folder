# W3C 标准中关于自定义 HTML 标签相关内容的简单汇总

浏览器对等非标准的自定义 HTML 标签，就像对待标准元素一样，只是没有默认的样式和行为。这种处理方式是写入 HTML5 标准的。

```html
<greeting>Hello!</greeting>
```

这类自定义标签支持通过 CSS 或 style 属性设置样式，实例为`HTMLUnkonwElement`，继承自`HTMLElement`，有通用的属性、方法和事件（如`onclick`）。

HTML 5 标准规定了自定义元素是合法的，W3C 就为自定义元素制定了一个单独的 Custom Elements 标准。它与其他三个标准放在一起：HTML Imports，HTML Template、Shadow DOM 统称为 Web Components 规范。

## Custom Elements

```html
<my-element>Custom Element</my-element>
```

Custom Elements 标准对自定义元素的名字做了限制。自定义元素的名字必须包含一个破折号（-）所以`<x-tags>`、`<my-element>`和`<my-awesome-app>`都是正确的名字，而`<tabs>`和`<foo_bar>`是不正确的。这样的限制使得 HTML 解析器可以分辨那些是标准元素，哪些是自定义元素。

还需要定义相应的类，类中可以扩展需要的属性和方法。

```javascript
class MyElement extends HTMLElement { 
    get testAttribute() {
        return this.getAttribute('test-attribute');
    }
    set testAttribute(value) {
        this.setAttribute('test-attribute', value);
    }
}
window.customElements.define('my-element', MyElement);
```

经过定义之后，`my-element` 标签的类型就不再是`HTMLUnkonwnElement`，而是`MyElement`了。

## HTML Import

自定义标签可以根据应用场景的语义来设置标签名。

```html
<share-buttons>
  <social-button type="weibo">
    <a href="...">微博</a>
  </social-button>
  <social-button type="weixin">
    <a href="...">微信</a>
  </social-button>
</share-buttons>
```

将上面内容保存为`share-button.html`文件，再通过`link`标签引入到其他页面进行复用。

```html
<link rel="import" href="share-buttons.html">
```

在页面中这样调用。

```html
<article>
  <h1>HTML Import Test</h1>
  <share-buttons/>
  ......
</article>
```

## Shadow DOM

Web components 的一个重要特性是"封装"————可以将标记结构、样式和行为隐藏起来，并与页面上的其他代码相隔离，保证不同的部分不会混在一起，可使代码更加干净、整洁。其中，Shadow DOM 是关键所在，它可以将一个隐藏的、独立的 DOM 附加到一个元素上。

Shadow DOM 允许将隐藏的 DOM 树附加到常规的 DOM 树中——它以 shadow root 节点为起始根节点，在这个根节点的下方，可以是任意元素，和普通的 DOM 元素一样。

一些 Shadow DOM 特有的术语需要了解：

* `Shadow host` 一个常规 DOM 节点，Shadow DOM 会被附加到这个节点上。
* `Shadow tree` Shadow DOM 内部的 DOM 树。
* `Shadow boundary` Shadow DOM 结束的地方，也是常规 DOM 开始的地方。
* `Shadow root` Shadow tree 的根节点。

使用`attachShadow`方法为元素添加 Shadow DOM。`open`表示添加后阴影节点可以通过`shadowRoot`属性被外部访问，而`closed`则不可以。

```javascript
let shadow = elementRef.attachShadow({mode: 'open'});
let shadow = elementRef.attachShadow({mode: 'closed'});
let myShadowDom = myCustomElem.shadowRoot;
```

可以使用通用的方法操作 DOM。

```javascript
let shadow = this.attachShadow({mode: 'open'});
var para = document.createElement('p');
shadow.appendChild(para);
```

可以为为 shadow DOM 添加一些 CSS 样式：

```javascript
var style = document.createElement('style');

style.textContent = `
.wrapper {
  position: relative;
}`
```

也可以使用外部样式。

```javascript
const linkElem = document.createElement('link');
linkElem.setAttribute('rel', 'stylesheet');
linkElem.setAttribute('href', 'style.css');
shadow.appendChild(linkElem);
```

## HTML Template

TEMPATE 是 HTML 原生标签，TEMPLATE 标签中的内容不会呈现给用户。在 javascript 中可以通过`content`属性获取 TEMPLATE 标签中的所有内容。

```html
<style type="text/css">
span {
  background-color: blue;/*设置页面所有span背景为蓝色，然而对shadow dom没什么卵用*/
}
</style>
<div id="container">没什么卵用的文字</div>
<template id="sample">
    <style type="text/css">
    span { color: red; }
    </style>
    <span>Hello World</span>
</template>

<script type="text/javascript">
let root = $('#container').attachShadow({mode:'open'});
let content = $('#sample').content.cloneNode(true);
root.appendChild(content);
</script>
```

TEMPLATE 的内容被塞到`container`中，字体颜色为红色，而不是蓝色。

TEMPLATE 标签中支持 SLOT 标签，`slot`是一个插槽，一个坑位，可以在 TEMPLATE 中定义坑位，然后宿主中的内容可以标记属于哪一个坑位，这样宿主的内容就会被正确地插入到TEMPLATE 所标记的位置去。

```html
<div id="container">
    没什么用的文字 1
    <span slot="main1">坑位1</span>
    <i slot="main2">坑位2</i>
    没什么用的文字 2
</div>
<template id="sample">Template Begin | 
    <slot name="main1">Slot 1</slot>
    <slot name="main2">Slot 2</slot>
    | Template End
</template>
```

处理结果为（实际上是 Shadow DOM 结构）：

```html
<div id="container">
    Template Begin | 
    <span slot="main1">坑位1</span>
    <i slot="main2">坑位2</i>
    | Template End
</div>
```

可以看到`没什么用的文字 1`、`没什么用的文字 2`、`Slot 1`、`Slot 2`都没有显示出来。其中前面两个被模板替换掉，后面两个本身就没有意义。

---

（本文完）
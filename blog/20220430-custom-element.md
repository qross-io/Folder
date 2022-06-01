# 定义自己的 HTML 标签

新版浏览器已经对自定义标签提供了支持，详细参照[W3C 标准中关于自定义 HTML 标签相关内容的简单汇总](/blog/20211111-custome-element.md)。刚重写了 [Popup 标签](/root.js/popup.md)，本文主要是记录自定义标签过程中的一些注意事项。

## 标签示例

标签名中必须包含中线`-`，是支持标准中明确要求，否则不能解析。

```html
<pop-up type="window">这是一个弹出框。</pop-up>
```

其他就必要用 Javascript 来实现了。

## 定义类

先上代码。

```javascript
class HTMLPopupElement extends HTMLElement {

    constructor() {
        super();
    }

    get type() {
        return this.getAttribute('type') || 'window';
    }

    set type(type) {
        this.setAttribute('type', type);
    }

    #hidings = [];

    get hidings() {
        return this.#hidings ?? [];
    }

    set hidings(hidings) {
        if (hidings instanceof Array) {
            this.#hidings = hidings;
        }
        else {
            this.#hidings = [hidings];
        }
    }

    open() {
        this.hidden = false;
    }

    close() {
        this.hidden = true;
    }
}

window.customElements.define('pop-up', HTMLPopupElement);
```

一些说明和注意事项。

* 类名可以使用通用格式命名，即`HTML`+标签名称+`Element`，HTML 中的所有标签都是这种格式，如多行文本框 HTMLTextAreaElement、链接 HTMLAnchorElement 等。
* 类必须继承`HTMLElement`，不能是其他子类，例如`HTMLDivElement`，不然会报错。
* 类中可以没有构造器 constructor，一般也不需要。构造器用于添加一些初始化逻辑。如果要使用构造器，则必须使用语句`super()`。
* 上例声明了属性`type`，可以将值保存在标签的自定义属性中，使用`getAttribute`和`setAttribute`进行获取和设置，适用于基本类型的属性，如字符串、数字等。
* 另外一个属性`hidings`，是一个数组，不方便将值保存在自定义属性中，可以将值保存在私有变量`#hidings`中。
* `open`和`close`用显示和隐藏弹出框。
* `window.customElements`方法用于定义自定义标签，这样才能让`pop-up`标签变成 HTMLPopupElement 对象而不再是 `HTMLUnkownElement`，就是这个方法要求自定义标签名字中间必须有下划线。

类结构及属性和方法只是为了说明编码过程，实际逻辑比这要复杂很多。

## 定义默认样式

一般情况下标签不定义样式是没法用的，Popup 标签本质是一个浮动层，所以要有自己的默认样式。有三种可选的方案，一是使用 Shadow DOM 将样式定义在标签内部，二是引入一个额外的样式文件，三是自动添加一些样式要页面上。

如果我们希望用户能控制样式，那么就不能使用 Shadow DOM 的方式。第二种方式每次使用都要手工引入外部文件，也不理想。所以还是自动附加样式到页面上，代码如下：

```javascript
if ($('head style') == null) {
    $('head').insertAdjacentHTML('afterBegin', '<style type="text/css"></style>');
}
$('head style').appendChild(document.createTextNode('pop-up { display: block; border: 1px solid #808080; padding: 1px; background-color: #FFFFFF; box-shadow: 2px 4px 6px #999999; position: absolute; z-index: 1001 }'));
```

## 扩展属性和方法

这时 HTMLPopupElement 就相当一个原生标签了，除了在类上进行属性和方法定义以外，可以使用多种方法对其进行属性和方法扩展，参见 [扩展 HTML 原生标签](/blog/20210731.md)。

## 如何调用

自定义标签就像原生标签一样使用，无论是`document.getElementById`方法还是`$`或`$$`，所有选择器都可以使用。

## 初始化的时机

很多初始化操作可以在构造函数中实现，但是自定义元素标准要求一些直接属性赋值的操作不能在构造函数中使用，需要自己在类外添加初始化逻辑。

* 私有属性或没有 getter 和 setter 的属性可以在声明时初始化。
* 样式属性可以通过 CSS 初始化。
* 有 getter 和 setter 的属性可以在 getter 逻辑中设置默认值。

除了定义默认样式，有时还需要对标签进行初始化。HTMLPopupElement 就要隐藏整个元素，并且定义模态层等操作。HTMLPopupElement 选择了两个时点进行初始化，一个时点是在页面加载完成时进行一次全局初始化，还一个时点是在`open`方法中，这主要应对页面还没有加载完成，用户就要使用 HTMLPopupElement，而且使用 HTMLPopupElement 只有`open`方法。

（本文完）


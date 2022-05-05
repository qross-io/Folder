
# HTML 标签自定义事件

HTML 支持为元素标签添加自定义事件。整个过程共分三步，定义事件、监听事件、触发事件。

```javascript
//定义事件
const event = new CustomEvent('hello', {
    detail: { 
        a: 1,
        b: 2
    }
});

//监听事件
$('#T').addEventListener('hello', function(e) {
    console.log(e.detail.a + ' ' + e.detail.b);
});

//触发事件
$('#T').dispatchEvent(event);
```

以上是网络上的标准版本，各步的解释和注意事项如下：

* `CustomEvent`用于创建自定义事件，主要在`dispatchEvent`方法中使用。所以，可以直接在`dispatchEvent`方法中定义。

```javascript
$('#T').dispatchEvent(new CustomEvent('hello', {
    detail: { 
        a: 1,
        b: 2
    }
}));
```

* `detail`是事件参数，即可以在触发时通过这个属性传递数据，不能是其他名字，也不能定义新的属性参数。其他还有三个布尔参数`bubbles`、`cancelable`、`composed`，基本用不到。`detail`里面就随便了，可以修改里面的值。

```javascript
event.detail.a = 3;
event.detail.a = 4;
```

* 在监听方法的事件函数里可以通过参数调取到传递的数据。上面的`e.detail.a`和`e.detail.a`。

* 事件名不包括`on`前缀，是`hello`而不是`onhello`。

* 事件函数只能通过`addEventListener`添加，而不是在元素标签属性上添加，添加了也不会触发，原生事件是可以的。

* `dispatchEvent`方法没有返回值，或永远返回`true`。原生事件同理，监听的事件没有返回值，有返回值也没什么用。只有通过属性定义的原生事件返回值有效，如`element.onclick = function(e) { return false; }`。

* `dispatchEvent`还可用于触发原生事件，如`element.dispatchEvent(new Event('click'))`。不仅可以触发`addEventListener`监听的原生事件，还可以触发在标签属性定义的原生事件。

```html
<button id="Button1" onclick="...">Button</button>
```

或

```html
$('#Button1').onclick = function(e) { ... }
```

root.js 库对以上逻辑进行了整合，可以通过元素标签的`defineCustomEvent`方法定义事件，通过`execute`方法执行事件，即可以执行标签属性事件，也可以执行监听事件，就像原生事件一样使用。

移除事件监听同样可以用`removeEventListener`， 不过要注意引用类型的问题。

```javascript
//需要保证添加和移除使用的同一个函数才可以。
let func = function(e) { ... };
$('#T').addEventListener('click', func);
$('#T').removeEventListener('click', func);

//这样就不行，因为两次引用的不是同一个内存地址。
$('#T').addEventListener('click', function(e) {...});
$('#T').removeEventListener('click', function(e) {...});
```

（本文完）

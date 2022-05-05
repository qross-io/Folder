# IMG 标签扩展

对 IMG 标签的扩展暂时不是为了图片服务······ 而是为了方便与后端交互数据。

```html
<img onerror+="post:/api/do-something" />
```

相关文件

```html
<script type="text/javascript" src="/root.js"></script>
<script type="text/javascript" src="/root.images.js"></script>
```

可以理解为由图片触发的服务器端的数据交互，主要用来记录数据。可以理解为是页面三个主要事件`onready`、`onpost`和`onload`（见 [Document 全局事件](/root.js/root.md#h1)）的补充。

## 扩展事件和属性

对 IMG 的主要扩展有 2 个[服务器端事件](/root.js/server.md)和 1 个调试相关属性。

* `onload+` 当图片加载完成后触发，对应客户端事件`onload`。
* `onerror+` 当图片加载失败时触发，对应客户端事件`onerror`。
* `debug` 当设置这个属性时，会在调试时输出后端请求的状态信息。

除此之外，原所有客户端事件现在都支持[精简事件表达式](/root.js/event.md)，所有原生属性都支持[增强属性](/root.js/plus.md)设置。

两个服务器端事件现在都有自己的 4 个状态事件，如`onload+success`、`onerror+exception`等，详见[服务器端事件监听](/root.js/server.md#h4)。

## onload+ 事件

IMG 标签的原有`onload`事件在图片加载完之后触发，如果没有设置`loading`属性为`lazy`，则一般在 document 的`onready`事件之前触发。而`onload+`事件在 document 的 `onpost`事件之后才触发。而且，也不能设置 `src` 属性，否则`onload+`事件不会触发，但是要设置`src+`属性。

```html
<img src+="/images/your-pic.jpg"  onload="do-someting" onload+="put:/api/do-more" onload+success="do-extra" />
```

各属性和事件分别解释如下：

* `src+`属性由[增强属性](/root.js/plus.md)提供支持，支持比原生属性更多的特性。如果用不到增强属性的特性，这里设置成原有属性的值（图片地址）即可。但是这个`src+`属性必须设置，而且不能设置成原生属性`src`。因为`src`属性只能触发客户端事件`onload`，而不能触发服务器端事件`onload+`。这个逻辑仅针对这个 IMG 标签，其他标签不存在这个问题。
* 当图片加载完成时，首先触发客户端事件`onload`。
* 服务器端事件`onload+`在客户端事件`onload`之后触发，一般用来向后端发送数据。
* 客户端事件`onload+success`及其他与`onload+`相关的状态事件在`onload+`事件执行完且数据结果从后端返回时触发。

## onerror+ 事件

IMG 标签原有的`onerror`当图片加载出错时触发，如地址不存在、网络错误、非法访问等。如果只是用来向后端发送数据，则只用`onerror+`即可，而不需要用`onload+`。

```html
<img onerror="do-something" onerror+="put:/api/do-more" onerror+exception+="do-extra" />
```

与`onload+`事件不同，`onerror+`事件不需要设置`src`和`src+`属性，如果设置了反而要设置一个不存在的地址才能触发`onerror`，不仅执行的顺序不对，也不会触发`onerror+`事件。客户端事件`onerror`在 document 的`onready`事件之后`onpost`和`onload`事件之前触发，而服务器端事件`onerror+`在 document 的`onpost`事件之后触发。

各属性和事件分别解释如下：

* 不需要也不能设置`src`和`src+`属性，因为我们的目的是让 IMG 标签出错。即使我们设置了一个错误的图片地址，也不会触发`onerror+`事件，而且控制台会报图片地址错误。不设置`src`属性则不会报错。
* 首先触发客户端事件`onerror`。
* 服务器端事件`onerror+`在客户端事件`onerror`之后触发。
* 服务器端事件`onerror+`的几个状态事件如`onerror+exception`在后端返回数据后触发。

## debug 属性

IMG 标签没有 INPUT 和 BUTTON 标签的各种提示信息，如果想获取`onload+`和`onerror`事件的状态信息，可以启用`debug`属性。

```html
<img debug onerror+="put:/api/do-something" />
```

在执行完成后会在控制台输出状态和返回的数据，主要用于调试，生产环境可以关闭。
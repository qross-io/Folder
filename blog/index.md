# 博客笔记

这里的内容多为老吴原创，都是开发过程中碰到的问题和一些开发经验，转载请注明出处。

### [Javascript 用数组实现队列和栈](/blog/20220508-array-queue-stack.md) &nbsp; /gray,16:2022-05-08/

Javascript 中没有队列和栈这两个数据结构，但是可以用数组配合相应的方法实现。

### [Javascript 中的变量使用总结](/blog/20220506-variable.md) &nbsp; /gray,16:2022-05-06/

Javascript 的变量声明和赋值其实不止三种方法，有很多种方式可以创建变量和赋值。

### [判断一个数组是不是另一个数组的子集](/blog/20220505-subset.md)  &nbsp; /gray,16:2022-05-05/

在 Javascript 中判断一个数组的元素是不是完全包含另一个数组的所有元素，有多种办法可以选。

### [HTML 标签自定义事件](/blog/20220503-custom-event.md) &nbsp; /gray,16:2022-05-03/

HTML 支持为标签添加自定义事件。由`CustomEvent`类定义事件，由`AddEventListener`方法添加事件监听，再由`dispatchEvent`方法触发事件。

### [大字符串拼接问题优化](/blog/20220501-string-builder.md) &nbsp; /gray,16:2022-05-01/

大字符串或者字符串多次拼接一定要用 StringBuilder！

### [定义自己的 HTML 标签](/blog/20220430-custom-element.md) &nbsp; /gray,16:2022-04-30/

W3C 已经提供了在 HTML 中自定义标签的接口，本文以一个弹出框为例简单介绍一下自定义标签的过程。

### [HTML 中 IMG 标签的 onload 和 onerror 事件](/blog/20220426-image.md) &nbsp; /gray,16:2022-04-26/

IMG 标签和 window 对象一样支持`onload`事件，也支持`onerror`事件，除了加载图片之外，还可以用这些事件做一些特殊的事。

### [HTML 元素的 hidden 属性不生效的问题](/blog/2022420-hidden.md) &nbsp; /gray,16:2022-04-20/

HTML 中的`hidden`属性用于显示和隐藏元素，但是当元素设置了`display`样式时，不论是在元素样式属性`style`中设置还是在 CSS 类中设置，则`hidden`属性就会失去作用。[阅读全文](/blog/2022420-hidden.md)

### [Javascript 选择器 $ 和 $$ 的坑](/blog/20220418-selector.md) &nbsp; /gray,16:2022-04-18/

原生选择器`$`和`$$`当在 HTML 标签属性中定义的事件中使用时会报错，如`onclick="$('#ID')...."`。[阅读全文](/blog/20220418-selector.md)

### [Javascript 中 replace 方法的一个坑](/blog/20220415-replace.md) &nbsp; /gray,16:2022-04-15/

在使用`replace`方法时，如果要替换的字符串中含有<code>$\`</code>时，会在替换时自动移除。[阅读全文](/blog/20220415-replace.md)

-- 20 --

更多内容请参阅左侧目录。
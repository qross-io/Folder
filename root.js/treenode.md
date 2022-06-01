# TreeNode 类

TreeNode 是构成 TreeView 的基本元素，TreeNode 也可以拥有自己的子节点，子节点还可以有子节点，所有 TreeNode 本身也是一个树。

## TreeNode 标签属性

几个最基本属性有：

* `icon` 节点在闭合状态的图标。
* `expandedIcon` 节点在展开状态的图标，如果不设置则同`icon`属性相同。
* `link` 节点的链接地址。
* `linkStyle` 链接样式，可选值`text`或`node`，默认为`text`。区别为`text`是把节点文本转成一个 A 链接，`node`是点击节点跳转链接。
* `name` 节点的名称，TreeNode 不是通过`id`来标识。如果没有设置，程序会自动添加一个。
* `target` 链接目标，同 A 标签的`target`属性。
* `text` 节点的显示文本。
* `tip` 节点的提示文字。
* `tipTitle` 节点提示文字的注释，鼠标划过`tip`时显示。
* `title` 节点的注释，鼠标划过时显示。
* `value` 节点的值，不可见。

* `data` 子节点的数据源。
* `template` 子节点使用的模板的名字。

* `cap` 节点上方的 HTML 内容，可选。
* `gap` 节点和其子节点之间的 HTML 内容，可选。
* `lap` 节点下方的 HTML 内容，可选。

* `indent` 节点缩进距离，单位像素。默认值为`-1`，表示从 TreeView 的`nodeIndent`属性继承。
* `padding` 节点的内间距，单位像素。默认值为`-1`，表示从 TreeView 的`nodePadding`属性继承。
* `spacing` 同级节点之间的间距，单位像素。默认值为`-1`，表示从 TreeView 的`nodeSpacing`属性继承。
* `childrenPadding` 与子节点之间的距离，单位像素。默认值为`-1`，表示从 TreeView 的`childrenPadding`属性继续。

每个节点的样式都可以单独设置，所有支持的样式属性如下：

`className` 节点的默认样式，如果不设置则从 TreeView 的`nodeClass`属性继承。
`textClass` 节点的文本样式，默认从 TreeView 的`nodeTextClass`属性继承。
`hoverClass` 鼠标划过节点时的样式，默认从 TreeView 的`nodeHoverClass`属性继承。
`selectedClass` 节点的选择后的样式，默认从 TreeView 的`selectedNodeClass`属性继续。
`selectedHoverClass` 节点在选择后鼠标划过的样式，默认从 TreeView 的`selectedNodeHoverClass`属性继续。
`editingBoxClass` 节点在编辑状态的文本框样式，默认从 TreeView 的`editingBoxClass`属性继承。
`iconClass` 节点的图标样式，默认从 TreeView 的`nodeIconClass`属性继承。
`expandedIconClass` 节点在展开状态的图标样式，默认从 TreeView 的`expandedNodeIconClass`属性继承。
`tipClass` 节点的提示内容的样式，默认从 TreeView 的`nodeTipClass`属性继承。
`capClass` 节点上方内容的样式，默认从 TreeView 的`nodeCapClass`属性继承。
`gapClass` 节点和子节点之间内容的样式，默认从 TreeView 的`nodeGapClass`属性继承。
`lapClass` 节点下方内容的新式，默认从 TreeView 的`nodeLapClass`属性继承。
`cutClass` 节点被剪切时的样式，默认从 TreeView 的`cutNodeClass`属性继承。
`dropClass` 有其他节点被拖放到当前节点时当前节点的样式，默认从`dropChildClass`属性继承。

用于控制节点的布尔属性有：

* `selectable` 节点是否可以被选中。
* `draggable` 节点是否可以被拖动，当 TreeView 的 dragAndDropEnabled 属性启动时生效。
* `droppable` 是否可以把其他节点放置为其子节点，当 TreeView 的 dragAndDropEnabled 属性启动时生效。
* `editable` 节点是否可以编辑。
* `visible` 节点是否可见。
* `expanded` 节点是否在第一次加载时展开。
* `selected` 节点是否默认被选择。
* `checked` 节点是否默认被选中。

## TreeNode 状态属性

这些属性不能通过标签属性设置，而且都是只读的，可以在 Javascript 中调用。

* `childNodes`或`children`所有子节点的数组。
* `editing` 节点是否正在编辑。
* `expanded` 节点是否已经展开。
* `expanding` 节点是否正在展开。
* `depth` 当前节点的深度，从`1`开始，`1`表示根节点。
* `firstChild` 获取当前节点的第一个子节点。
* `hasChildNodes` 判断节点是否有子节点。
* `index` 获取当前节点的同级索引位置，从`0`开始。
* `lastChild` 获取当前节点的第一个子节点。
* `loaded` 节点是否已经加载完成。
* `loading` 节点是否正在加载中。
* `parentNode` 获取当前节点的父节点，如果是根节点，则返回`null`。
* `path` 当前节点从根节点到当前节点的完整的`name`路径，名称之间使用点`.`隔开。如`n1.n2.n3`，其中`n1`是根节点，`n2`是`n1`的子节点，`n3`是`n2`的子节点。这个路径在一些场景下会用到。
* `previousSibling` 获取当前节点的上一个同级节点。
* `nextSibling` 获取当前节点的下一个同级节点。
* `treeView` 节点所属的 TreeView。

下面是与节点元素相关的属性，这些属性除非自己定义 TreeNode，否则用不到。

* `primeDiv` 节点的 DIV 元素。
* `majorElement` TreeNode 的节点元素，根据`nodeCellStyle`的设定会有所不同，可能是 DIV 或 TD 元素。
* `tableElement` TreeNode 的主 TABLE 元素。
* `childrenDiv` 子节点的 DIV 元素。
* `burlImage` 展开和闭合节点的图标 IMG 元素。
* `checkBoxElement` 复选框 IMG 元素。
* `iconCell` 节点图标所在的 TD 元素。
* `textCell` 节点文本所在的 TD 元素。
* `linkAnchor` 节点链接的 A 元素。
* `tipCell` 节点提示的 TD 元素。
* `capDiv` 节点上方内容所在的 DIV 元素。
* `gapDiv` 节点和其子节点之间内容所在的 DIV 元素。
* `lapDiv` 节点下方内容所在的 DIV 元素。

## 方法

节点操作相关方法（按功能排序）。

* `expand()` 展开节点。
* `collapse()` 闭合节点。
* `toggle()` 切换展开和闭合状态。
* `select()` 选择节点。
* `deselect()` 取消选择节点。
* `check()` 复选节点。
* `uncheck()` 取消复选节点。
* `show()` 显示隐藏的节点。
* `hide()` 隐藏节点。
* `reload()` 重新加载节点的数据，即节点的子节点。
* `remove()` 移除自己。
* `edit()` 开始编辑文本。
* `cut()` 剪切节点。
* `copy()` 复制节点。
* `paste()` 粘贴节点。
* `decrease(decrement = 1)` 如果节点的`tip`是数字，则将数字递减`decrement`。
* `increase(increment = 1)` 如果节点的`tip`是数字，则将数字递增`increment`。

子节点或元素相关方法：

* `appendChild(treeNode)` 附加子节点。
* `appendChildElement(element)` 附加子元素，`element`参数也支持 HMTL 字符串。
* `insertFront(treeNode)` 将`treeNode`添加为第一个子节点。
* `insertElementFront(element)` 将`element`添加为子节点中的第一个元素。
* `insertBefore(treeNode, referenceNode)` 在子节点`referenceNode`之前插入子节点`treeNode`。
* `insertElementBefore(treeNode, referenceNode)` 在子节点`referenceNode`之前添加元素`element`，`element`支持 HTML 字符串。
* `insertAfter(treeNode, referenceNode)` 在子节点`referenceNode`之后添加子节点`treeNode`。
* `insertElementAfter(element, referenceNode)` 在子节点`referenceNode`之后添加元素`element`，`element`支持 HTML 字符串。
* `removeChild(treeNode)` 移除子节点`treeNode`。
* `removeChildElement(element)` 移除子元素`element`。
* `appendTo(parentNode)` 将节点附加到`parentNode`的子节点。
* `insertToBefore(parentNode, referenceNode)` 将节点插入到`parentNode`的子节点`referenceNode`的前面。
* `insertToAfter(parentNode, referenceNode)` 将节点插入到`parentNode`的子节点`referenceNode`的后面。

其他方法：

* `if(exp)` 接受一个函数或布尔表达式或表达式字符串。检查是否满足指定的条件，满足返回自己，不满足返回`null`。`exp`如果是一个函数，需返回布尔值，参数传递当前节点；`exp`如果是一个字符串，则先进行运算再判断，字符串中支持使用`this`关键字，指向当前节点；也可以是布尔值或布尔表达式。
    ```javascript
    $s('#TreeView1').selectedNode?.if(node => node.name == 'Node1')?.increase();
    $s('#TreeView1').selectedNode?.if(function(){ return this.name == 'Node1'; })?.increase();
    $s('#TreeView1').selectedNode?.if("this.name == 'Node1'")?.increase();
    ```
* `of(parentNode)` 设置节点的父级元素。
* `getAttribute(attr)` 获取节点的属性`attr`的值。
* `setAttribute(attr, value)` 设置节点的属性`attr`的值为`value`，一般为自定义属性。

## 节点文本

节点文本是节点的说明，也是一个节点最基本的属性。节点文本支持使用`text`属性或 TEXT 标签进行设置。一般纯文本使用`text`属性，当使用 HTML 内容时使用 TEXT 标签，理论上 TEXT 标签中可以使用任意的 HTML 内容。

```html
<treeview id="TreeView1">
    <treenode text="Groups"></treenode>
    <treenode text="Settings"></treenode>
    <treenode>
        <text><span class="bold gray">About</span></text>
    </treenode>
</treeview>
```

## 节点图标

节点图标支持图片、Iconfont和任意的 HTML 代码，通过`icon`属性或 ICON 标签进行设置。如果使用`icon`属性，当属性值以`.jpg`、`.png`或`.gif`结尾时，会自动识别为图片；当属性值以`icon-`开头时，会自动识别为 Iconfont。如果`icon`属性不能满足要求，可以使用 ICON 标签进行设置，理论上 ICON 标签中可以使用任意的 HTML 代码。如下例：

```html
<treeview id="TreeView1">
    <treenode text="Groups" icon="/images/group.jpg"></treenode>
    <treenode text="Settings" icon="icon-setting"></treenode>
    <treenode text="About">
        <icon><i class="iconfont icon-about"></i></icon>
    </treenode>
</treeview>
```

节点有两种状态，一种是闭合或折叠状态，另一种是展开状态。相应的，可以分别设置两种状态下的图标，默认的`icon`属性或 ICON 标签表示闭合状态下的图标，可以使用`expanded-icon`属性或 EXPANDED-ICON 标签设置展开状态下的图标（中横线可以省略）。

```html
<treeview id="TreeView2">
    <treenode text="Projects" icon="/images/folder-close.png" expanded-icon="/images/folder-open"></treenode>
    <treenode text="Trash">
        <icon><i class="iconfont icon-recycle-full"></i></icon>
        <expanded-icon><i class="iconfont icon-recycle-empty"></i></expanded-icon>
    </treenode>
</treeview>
```

一般图标选取或设置为 16 像素大小视觉效果最佳。

---
参考链接

* [TreeView 树形目录](/root.js/treeview.md)
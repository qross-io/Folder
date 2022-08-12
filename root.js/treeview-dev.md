# TREEVIEW 开发文档

## TREEVIEW

### Template 逻辑

对于根节点来说，可以只有静态节点，也可以只有动态节点，也可以二者混合。

只有静态

```html
<treeview>
    <treenode>...</treenode>
    <treenode>...</treenode>
</treeview>
```

只有动态

```html
<treeview>
    <template as="array" data="...">
        <treenode>...</treenode>
    </template>
</treeview>
```

混合

```html
<treeview>
    <treenode>...</treenode>
    <template as="array"  data="...">
        <treenode>...</treenode>
    </template>
    <treenode>...</treenode>
</treeview>
```

所有模板（包括 TREENODE 使用的模板）必须定义在 TREEVIEW 标签下，模板定义位置与实际数据显示位置无关。

模板数据显示位置根据 SLOT 标签的位置确定，一个 SLOT 标签加载一个 TEMPLATE 模板的数据，不同的 SLOT 可以对应同一个 TEMPLATE。

SLOT 的`for`或`template`属性指定模板的名字或 ID，`data`属性指定数据源，将应用在模板上。

TEMPLATE 标签上不能设置`data`属性，一是有可能不同的节点或 SLOT 会使用同一个模板，二是设置`data`数据后 TEMPLATE 标签会自动加载而不是由 TREEVIEW 或 TREENODE 触发加载。

当根节点只有模板数据或模板数据加载在其他静态节点后面时，可以省略 SLOT 标签，但是需要在 TREEVIEW 上定义`template`和`data`属性。

根节点支持多个模板，即设置多个 SLOT。

使用 TEMPLATE 标签而不是 FOR 标签是因为 TEMPLATE 支持重新加载。

属性`data`只支持一个数据源，支持调用 TREEVIEW 属性参数，例如`{value}`。

多数据源混合示例：

```html
<treeview>
    <treenode name="n1">...</treenode>
    <slot for="t1"></slot>
    <treenode name="n2">...</treenode>
    <slot for="t2"></slot>
    <treenode name="n3">...</treenode>
    <template name="t1" as="array" data="...">
        <treenode>...</treenode>
    </template>
    <template name="t2" as="array" data="...">
        <treenode>...</treenode>
    </template>
</treeview>
```

所有模板
template 属性
data 属性
slot 标签，如果没有就创建一个

## TREENODE

### name 逻辑

**name** 是 TREENODE 的唯一标识，在同级节点中名称需唯一，在不同的父级节点下名称可以重复，可以以数字做为 name 的值，但不能包括小数点。

name 默认值为`TreeNode_`加 9 位随机字符。

name 是路径的一部分，路径包含从根节点到当前节点的所有节点的 name，name 之间使用小数点分隔。

### text 逻辑

TEXT 支持普通文字和复杂的 HTML 内容。普通文本可以通过`text`属性设置，复杂的 HTML 内容可以通过`<text>`子标签来设置。

公开变量：

* `text` 获取或设置 text 的内容。
    初始化前，获取 TREENODE 的`text`属性或`<text>`标签的`innerHTML`。
    初始化后，获取 textCell 的 innerHTML，当 link 不为空时则为 A 链接内的文字。
* `textCell` 指向 text 所在的单元格，只读。
* `textClass` 设置文本的样式，作用在`textCell`上，可以通过 TREEVIEW 的`nodeTextClass`进行全局设置。
    也可以在`<text>`通过`class`属性定义，

私有变量：

* `#textCell` 指向 text 所在的单元格。

### value 逻辑

VALUE 对于 TREENODE 来说不可见，只能在属性上定义，一般不会设置过于复杂的值。

### link 逻辑

LINK 只在属性上定义，可选`link`或`href`属性，只要定义了，就附加在文本上作为一个链接。

属性`linkMode`是只读属性，只能在标签属性上定义，默认从 TREEVIEW 继承，默认值为`text`，另外可选`node`。

属性`linkTarget`默认值为`_blank`，不能在当前页打开链接，默认从 TREEVIEW 继承 。

无论 text 中的值是文本文字还是 HTML 标签，只要设置了 link 属性，则将 A 标签包围在 text 属性周围。所以不建议在复杂 text 内容的节点上设置链接。

### icon 和 expanded-icon 逻辑

ICON 支持设置图片、IconFont、文字或 HTML 内容。

ICON 可以在 TREENODE 的`icon`或`expanded-icon`属性上进行设置，如果特别复杂时，可以在子标签`<icon>`或`<expanded-icon>`中进行设置。

公开变量

* `icon` 获取或设置 icon 的内容，图片地址和 IconFont 将会自动转化成 HTML 内容。
    初始化前，获取 TREENODE 的`icon`属性或`<icon>`标签的`innerHTML`。
    初始化后，获取 iconCell 的`origin`属性或`iconCell`中的值。
    获取生成后的 HTML 内容或样式等信息可以通过 iconCell
    初始化前，设置 icon，如果有则优先`<icon>`标签，否则设置在`icon`属性中。
    初始化后，转化成图片或图标再设置在`iconCell`中，原始值保存在`origin`中（如果不一致）。
* `expandedIcon` 获取或设置`expanded-icon`的内容。其他同`icon`。
* `iconCell` 获取 icon 所在的单元格`<td>`，只读属性。
* `iconClass` 节点图标的样式，作用在 collapsed 标签上，可以在 TREEVIEW 上通过属性`nodeIconClass`进行全局设置。
* `expandedIconClass` 展开节点的图标样式，作用在 expanded 标签 上，可以在 TREEVIEW 上通过属性`expandedNodeIconClass`进行全局设置。

私有变量

* `#iconCell` 指向 icon 所在的单元格。

切换 icon 即切换两个 ICON 所在的单元格的显示和隐藏。如果没有设置`expanded-icon`，则不生成 expanded-icon 单元格，也不会切换显示隐藏。

初始化后的`iconCell`标签结构如下，其中`collpased`标签保存闭合状态下的图标，`expanded`标签保存展开状态下的图标，如果不设置展开状态的图标，则`expanded`标签内容为空。

```html
<td sign="ICON">
    <collapsed class="icon-class">...</collapsed>
    <expanded class="expanded-class">...</expanded>
</td>
```


### cap / gap / lap 逻辑

三个标签分别对应 TREENODE 的上中下。

`cap` 是节点上方的自定义内容。
`gap` 是节点和其子节点之间的自定义内容。
`lap` 是节点下方的自定义内容，在所有子标签下方。

三个逻辑相同，以其中一个说明。

在`<cap>`标签中定义内容，而且可以在`<cap>`标签上设置类和样式，也可能设置其他属性，也可以设置自定义属性。

```html
<cap style="..." class="...">...</cap>
```

标签`<cap>`在初始化后变为`<treenode-cap>`标签。

私有属性

* `#capElement` 指向初始化后的`<treenode-cap>`标签。
* `#capHTML` 整个`<cap>`标签的`outerHTML`。

公开属性

* `cap` 与`#capElement`对应，可以访问初始化后的`<treenode-cap>`标签，执行获取或设置属性等操作。

已移除`capClass`属性，但在 TREEVIEW 上可以设置`nodeCapClass`，以可以让所有 CAP 都设置同一个样式。


### 节点的样式逻辑

nodeClass 和 className
nodeHoverClass 和 hoverClass
selectedNodeClass 和 selectedClass
selectedNodeHoverClass 和 selectedHoverClass
nodeTextClass 和 textClass

nodeCellStyle 为`text`时，不建议设置 textClass 样式。实际为叠加关系。

`<treenode text-class="...">`和`<text class="...">`优先前者，后者设置无效。如果前者不设置，则使用后者。其他样式逻辑相同。


### Template 逻辑


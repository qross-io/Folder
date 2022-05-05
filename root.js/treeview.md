# TreeView 树形目录

TreeView 是 root.js 库的第一个组件，也是历史最长的一个组件，第一个版本诞生于 2001 年，当前已经是第 8 个版本。作者还曾经为这个组件单独申请过`treeview.org`的域名。这个组件也是 root.js 库中最复杂的组件，当前版本不包含公共功能已经超过 4000 行代码，之前的第 6 版独立运行版超过 6000 行代码。

TreeView 目录的主要功能有：

节点相关特性：

* 支持设置节点的文本、值、提示文字及各种自定义属性，理论上支持任意的节点内容。
* 支持将图片、Iconfont 和任意 HTML 代码作为节点图标。
* 支持三态复杂框。
* 支持节点文本编辑。
* 支持节点复制、剪切和粘贴，用键盘就可以操作。
* 支持节点拖拽，可以设置将节点拖放到任意地方，如不同 TreeView 之间的拖拽，将节点拖放到其他元素上。
* 支持在节点之间穿插任意的 HTML 内容。
* 多个属性用于设置节点样式，如间距、选中风格等。
* 丰富的样式类属性用于设置节点各部分的样式。

数据相关特性：

* 支持 Json 接口数据，按需加载，无限级别。
* 通过模板来设置节点输出，任意接口格式都可以映射。
* 也支持静态 Json 数据。

交互相关特性：

* 超过 20 个事件用于控制与用户的各种交互。

因功能太多，相关文档和示例会慢慢补全。

## 使用 TreeView

需要引用的相关文件如下：

* `root.js` 基础库。
* `root.model.js` 数据加载模型。
* `root.treeview.js` 核心功能。
* `css/root/treeview.css` 默认样式文件。

另外，根据功能不同，需要`images`目录下的一些图片，使用时会自动加载，无需引用。

## 开始使用

使用标签 TREEVIEW 来表示整个树，使用[标签 TREENODE](/root.js/treenode.md) 表示单个节点。一个简单的静态树是这样的：

```html
<treeview id="TreeView1">
    <treenode text="根节点 1">
        <treenode>子节点 1</treenode>
        <treenode>子节点 2</treenode>
    </treenode>
    <treenode text="根节点 2"></treenode>
    <treenode text="根节点 3"></treenode>
</treeview>
```

请参阅 [TreeNode 文档](/root.js/treenode.md)获取关于节点的信息，更多的示例见下文示例章节。

## TreeView 标签属性

标签属性在 TREEVIEW 元素上定义，用于树形目录的初始化。标签属性也可以在 Javascript 中调用，但需要改成 Camel 命名规则，而且标签属性都是只读的。例如属性`lines-visible`，在 Javascript 中使用时需要改成`linesVisible`。

* `id`或`name` TreeView 唯一标识。

节点链接相关属性：

* `linkStyle` 节点链接的样式，可选`text`和`node`，默认`text`。`text`表示使用默认的链接样式，即将节点文本转成 A 标签，`node`表示点击节点时跳转。
* `target` 链接的默认目标。

数据相关属性：

* `data` 根节点的数据源，一般为接口地址，请参见[数据相关属性](/root.js/data.md)。
* `await` 等待其他组件加载完成再加载 TreeView，请参见[数据相关属性](/root.js/data.md)。
* `template` 根节点模板的名字，请参见“数据模板”一节。

TreeView 中的构成元素相关设置：

* `burlsVisible` 是否显示节点展开和闭合图标，默认值为`true`。
* `linesVisible` 是否显示分支虚线，默认值为`false`。
* `iconsVisible` 是否显示节点图标，默认值为`true`。
* `checkBoxesVisible` 是否显示复选框，默认值为`false`。

节点样式、缩进和间距设置相关属性：

* `nodeCellStyle` 定义鼠标划过或选择节点时的样式范围，可选值`text`和`row`，即选中节点时是只选中文本还是选中整个节点，默认值为`text`。
* `nodeIndent` 每级 TreeNode 的缩进距离，默认值为`16`，单位像素。
* `nodePadding` 节点内对象（如文本、图标）与节点外框之间的距离，默认值为`2`，单位像素。
* `nodeSpacing` 两个同级节点之间的间距，默认值为`0`，单位像素。
* `childrenPadding` 父节点与子节点之间的距离，默认值为`0`，单位像素。

操作和预设选项：

* `expandOnSelect` 是否在选择节点时展开闭合状态的子节点，默认值为`true`。
* `collapseOnSelect` 是否在选择节点时关闭展开状态的子节点，默认值为`false`。
* `expandDepth` 默认展开的节点深度，`1`为根节点，`0`为展开所有，默认值为`1`。
* `preloadDepth` 默认加载的节点深度，`1`为根节点，`0`为加载所有（强烈不建议），默认值为`1`。
* `pathToSelect` 默认选择的节点，格式`n1.n2.n3`，即节点的完整路径。
* `pathsToCheck` 在启用复选框时，默认选中的项，可设置多个节点，格式为`n1.n2.n3,n1.n2.n4,...`。

拖放选项，通过拖放功能可以实现移动、排序等功能，详见下面的章节：

* `dragAndDropEnabled`，是否启用拖放，默认值为`false`。
* `dragNodeEnabled` 是否可以拖动节点，默认值为`true`。
* `dropSpacingEnabled` 是否可以拖放到节点和节点之间，默认值为`true`。
* `dropChildEnabled` 是否可以放置到其他节点中， 默认值为`true`。
* `dropRootEnabled` 是否可以放置为根节点, 默认`true`。
* `externalDropTargets` 可以从 TreeView 把节点拖放到其他类型元素上，这个属性可以设置其他元素的选择器。

TreeView 样式类相关的属性：

* `className` 整个 TreeView 的样式类，默认值为`treeView-default-class`。
* `nodeClass` 节点的默认样式，默认值为`treeNode-default-class`。
* `nodeHoverClass` 鼠标划过节点的样式，默认值为`treeNode-default-hover-class`。为了配合事件`onNodeHover`，不建议使用伪类`:hover`。
* `selectedNodeClass` 被选择状态下的节点样式，默认值为`treeNode-default-selected-class`。
* `selectedNodeHoverClass` 鼠标划过被选择状态下的节点样式，默认值为`treeNode-default-selected-hover-class`。为了配合事件`onNodeHover`，不建议使用伪类`:hover`。
* `nodeTextClass` 节点文本的样式，默认值为空。
* `editingBoxClass` 编辑节点文本时文本框的样式，默认值为空。
* `nodeIconClass`，节点图标样式，默认值为空。
* `expandedNodeIconClass` 展开节点的图标样式，默认值为空。
* `nodeTipClass` 节点提醒文字样式, 默认值为空。
* `nodeCapClass` 节点上方元素的样式，默认值为空。
* `nodeGapClass` 节点和其子点之间的空隙元素的样式，默认值为空。
* `nodeLapClass` 节点下方元素的样式，默认值为空。
* `cutNodeClass` 剪切或移动中的节点样式，默认值为空， 透明度`0.5`。
* `dropChildClass` 当拖动的节点 Hover 到可放置节点时可放置节点的样式，默认值为空。
* `dropTargetClass` 外部拖放目标的默认样式。
* `dropTargetHoverClass` 外部拖放目标拖放划过时的样式。

键盘选项：

* `nodeEditingEnabled` 是否启用节点文本编辑，默认值为`false`。
* `keyboardNavigationEnabled` 是否启用键盘导航，默认值为`true`。对应的按钮为：展开`→`、闭合`←`，上一个节点`↑`，下一个节点`↓`，打开链接`Enter`，编辑节点`Shift+E`。
* `keyboardCutCopyPasteEnabled` 是否启用节点复制剪切和粘贴，默认值为`false`，启用时`Ctrl+C`、`Ctrl+X`，`Ctrl+V`可用。

TreeView 使用到一些图片，这些图片存放在源码库的`images`目录下。这些图片属性也可以自定义：

* `imagesBaseURL` 设置图片的存放目录，默认是`root.treeview.js`文件同级的`images`目录。
* `expandImageURL` 指示节点可以被展开的图标, 一般是一个`+`号，默认值为`burl_0a.gif,burl_0b.gif`，第二个图片是鼠标划过时显示的图片。
* `collapseImageURL` 指标节点可以被闭合的图标, 一般是一个-号，默认值为`burl_1a.gif,burl_1b.gif`，第二个图片是示鼠标划过时显示的图片。
* `contentLoadingImageURL` 正在载入状态图标, 在展开数据节点时显示，默认值为`spinner.gif`。
* `nonExpandableImageURL` 当无子节点时显示的图标，默认值为`blank.gif`，即空白图片。

## TreeView 状态属性

区别于标签属性，这些属性不能在标签上定义，但可以在 Javascript 中调用。这些属性都 TreeView 的状态或过程相关，一般都是只读的。

* `checkedNodes` 获取 TreeView 中被选中`checked`的所有节点，是一个数组，如果没有节点被选中，则返回空数组。
* `childNodes`或`children` 所有根节点的数组。
* `container` 显示 TreeView 内容的元素，在 TREEVIEW 标签之前由程序创建的一个 DIV 标签。
* `editingNode` 正在编辑状态的节点，如果没有节点被编辑，则返回`null`。
* `element` 保存配置信息的元素，即 TREEVIEW 标签。
* `firstChild` 第一个子节点。
* `hasChildNodes` 判断是否根节点。
* `lastChild` 最后一个子节点。
* `loaded` 标识 TreeView 根节点是否已经加载完成。
* `path` 被选择节点的路径，如果没有节点被选择，则返回`null`。
* `selectedNode` 当前选择`selected`的节点，如果没有节点被选择，则返回`null`。
* `text` 被选择节点的文本，如果没有节点被选择，则返回`null`。
* `value` 被选择节点的值，如果没有节点被选择，则返回`null`。

## TreeView 事件

因为事件名称的单词较多，所以 TreeView 的事件没有按照 HTML 标准（全部小写）来设置，目的是为了方便识别。但是在设置时仍支持全部小写，如`onnodeselected="..."`。

* `onBeforeLoad` 加载之前触发，事件函数`function() { }`。
* `onBeforeReload` 重新加载之前触发，事件函数`function() { }`。
* `onLoaded` 加载完成后触发，事件函数`function() { }`
* `onReloaded` 每次重新加载之后触发，事件函数`function() { }`
* `onEveryLoaded` 每次加载完成之后触发但不包含`lazyLoad`，事件函数`function() { }`。
* `onLazyLoaded` 每次增量加载之后触发，事件函数`function() { }`。
* `onNodeExpanded` 节点展开后触发，事件函数`function (expandedNode) { }`，参数`expandedNode`为刚刚被展开的节点。
* `onNodeCollapsed` 节点关闭后触发，事件函数`function (colapsedNode) { }`，参数`colapsedNode`为刚刚被闭合的节点。
* `onNodeLoaded` 每次节点加载完之后触发，事件函数`function(loadedNode, data) { }`，参数`loadedNode`为刚刚加载完数据的节点，`data`为加载返回的数据。
* `onNodeHover` 当鼠标划过节点时触发，事件函数`function (hoverNode) { }`，参数`hoverNode`为鼠标划过的那个节点。支持通过`return false`取消操作。
* `onNodeIconClick` 节点图标被点击时触发，事件函数`function(clickedNode) { }`，参数`clickedNode`为被点击的那个节点。
* `onNodeSelected` 当选择节点时或选择节点改变后触发，事件函数`function (selectedNode) { }`，参数`selectedNode`为刚刚被选择的那个节点。
* `onSelectedNodeClick` 当选中的节点被再次点击时触发，事件函数`function (selectedNode) { }`，参数`selectedNode`为被选择的节点。
* `onNodeCheckChanged` 当某个节点选中状态改变后触发，事件函数`function (node) { }`，参数`node`为选中状态变化的那个节点。
* `onNodeEdit` 当某个节点开始编辑时触发，事件函数`function (editingNode) { }`，参数`node`为正在编辑的那个节点。支持通过`return false`取消操作。
* `onNodeTextChanged` 节点文本更改后触发，事件函数`function (editedNode) { }`，参数`editedNode`为被编辑的节点。
* `onNodeCopied` 键盘事件，节点被复制后触发，事件函数`function (copiedNode) { }`，参数`copiedNode`为被拷贝的节点。
* `onNodeMoved` 键盘事件，节点被移动后触发事件函数，事件函数`function (movedNode) { }`，参数`copiedNode`被移动的节点。
* `onNodeDragStart` 节点开始被拖拽时触发，事件函数`function (draggedNode) { }`，参数`droppingNode`为被拖拽的节点。
* `onNodeDragEnd` 节点结束拖拽时触发，事件函数`function (draggedNode) { }`，参数`droppingNode`为被拖拽的节点。
* `onNodeDragOver` 节点拖放到其他节点上时触发，事件函数`function (droppingNode) { }`，参数`droppingNode`为拖放划过的节点。
* `onNodeDropped` 节点被拖放完成后触发，事件函数`function (droppedNode) { }`，参数`droppedNode`为被拖放的节点。
* `onNodeExternalDragOver` 节点被拖放到外部元素时，拖动进入目标元素时触发，事件函数`function (target) {}`，参数`target`为目标元素。
* `onNodeExternalDragLeave` 节点被拖放到外部元素时，拖动离开目标元素时触发，事件函数`function (target) {}`，参数`target`为目标元素。
* `onNodeExternalDropped` 节点被拖放到外部元素后触发 (在事件函数中可通过`this.selectedNode`得到被拖放的节点)，事件函数`function (target) { }`，参数`target`为接受拖放的元素。
* `onExternalElementDragOver` 外部元素拖动到节点后触发，事件函数`function (hoverNode) { }`， 参数`hoverNode`为正在拖放划过的节点。
* `onExternalElementDropped` 外部元素拖放到节点后触发，事件函数`function (droppedNode) { }`，参数`droppedNode`为接受拖放的节点。
* `onNodeNavigate` 点击节点上的链接时触发，`javascript:void(0)`不算链接，事件函数`function (node) { }`，参数`node`为目标节点。
* `onNodeRemove` 在节点删除前触发，事件函数`function (node) { }`，参数`node`为目标节点。支持通过`return false`取消操作。

## TreeView 方法

* `appendChild(treeNode)` 添加一个根节点。
* `checkAll()` 选中`checked`所有节点。
* `checkNodesByPaths(paths)` 根据指定的一个或多个路径选中节点。
* `collapseAll()` 折叠所有节点。
* `expandAll()` 展开所有节点。
* `expandAllNodeByNode()` 一个一个的展开所有节点，当 TreeView 要加载的数据非常多时可以使用这个方法。
* `getNodeByName(name)` 根据节点名字得到指定的节点。
* `hide()` 隐藏整个 TreeView。
* `insertAfter(treeNode, referenceNode)` 在参考节点`referenceNode`之后添加一个新节点`treeNode`。
* `insertBefore(treeNode, referenceNode)` 在参考节点`referenceNode`之前添加一个新节点`treeNode`。
* `insertFront(treeNode)` 在 TreeView 的所有根节点最前面添加一个新的根节点。
* `loadAll()` 加载所有节点的数据但不展开。
* `loadAllNodeByNode()` 一个节点一个节点的加载所有数据但不展开，当要加载的数据非常多时可以使用这个方法。
* `reload()` 重新加载 TreeView 的数据。
* `removeAll()` 移除所有根节点。
* `removeChild(treeNode)` 移除指定的根节点。
* `selectNodeByPath(path)` 根据节点的完整路径（由点`.`分隔的节点名称组成）选择这个节点，请参阅 [TreeNode 文档](/root.js/treenode.md)中关于路径`path`的说明。
* `show()` 如果 TreeView 是隐藏状态，则显示。
* `uncheckAll()` 取消选中`checked`所有节点。

## 选择器

可使用通用选择器`$s`通过 ID 得到指定的 TreeView，如`$s('#TreeView1')`，TreeView 的类型为`HTMLTreeViewNode`。

## 拖放操作

拖放操作的目标是节点，节点可以被拖放到内部的节点位置或其他节点的子节点，也可以被拖放到外部元素或其他树形目录上，外部元素只能被拖放到节点上。通过给定的几个与拖放有关的属性，可以组合成不同的操作。

* 启用拖放 `drag-and-drop-enabled="true"`，只有这个属性为`true`其他与拖放相关的属性才生效。
* 只能将节点拖放到外部元素或其他 TreeView 上 `drop-spacing-enabled="false" drop-child-eabled="false"`。
* 只接收外部元素的拖放 `drop-spacing-enabled="false" drag-node-enabled="false"`。
* 不可以将节点拖放为某一元素的子节点，仅同级排序 `drop-child-enabled="false" drop-spacing-enabled="true"`。
* 不可以将节点拖放到根节点下（即根节点的 Spacing 中） `drop-root-enabled="false"`。

## 更多示例

一个稍微复杂的树，包含树的一些属性设置、从接口加载数据及模板节点等。

```html
<treeview id="Catalog" icons-visible="yes" node-cell-style="row" link-style="node" target="WorkFrame"
            drag-and-drop-enabled="no" drag-node-enabled="no" drop-spacing-enabled="no" drop-root-enabled="no" node-indent="12" node-padding="5" node-spacing="1"
            node-class="catalog-item" node-editing-enabled="no" node-hover-class="catalog-item-hover"
            selected-node-class="catalog-item-selected" selected-node-hover-class="catalog-item-selected-hover">                
    <treenode name="users" template="departments" link="/user/users?department=" text="# all-users #" data="/api/user/departments">
        <icon><i class="iconfont icon-team"></i></icon>
        <tip>(<%=SELECT COUNT(0) FROM qross_users -> FIRST CELL %>)</tip>
    </treenode>
    <treenode name="teams" text="# teams #" template="teams" link="/user/teams" data="/api/user/teams">
        <icon><i class="iconfont icon-team"></i></icon>
        <tip>(<%=SELECT COUNT(0) FROM qross_teams -> FIRST CELL %>)</tip>
    </treenode>                
    <treenode name="roles" text="# roles-rules #" link="/user/roles-rules">
        <icon><i class="iconfont icon-Ruler"></i></icon>
        <% IF @role == 'master' THEN %>
            <treenode name="role" text="# keeper-rules #" link="/user/keeper-rules">
                <icon><i class="iconfont icon-Ruler"></i></icon>
            </treenode>
        <% END IF %>
    </treenode>                
    <treenode name="records" visible="false" text="# operating-records #">
        <icon><i class="iconfont icon-sousuo"></i></icon>
    </treenode>
    <template name="departments">
        <for in="@:/">
            <treenode name="department_@[id]" value="@[department]" link="/user/users?department=@[department]">
                <icon><i class="iconfont icon-addteam"></i></icon>
                <text>@[department]</text>
                <tip class="gray f12">(@[amount])</tip>
            </treenode>
        </for>
    </template>
    <template name="teams" as="list">                    
        <treenode name="team_@[id]" value="@[id]" text="@[team_name]" link="/user/member?id=@[id]">
            <icon><i class="iconfont icon-addteam"></i></icon>
            <tip class="gray f12">(@[members])</tip>
        </treenode>                    
    </template>
</treeview>
```



---
参考链接

* [TreeNode 树形节点](/root.js/treenode.md)
* [root.js 基础库](/root.js/root.md)
* [Model 数据加载模型](/root.js/model.md)


# TreeNode 类

文档正在完善中...

## 属性

* `text` 节点的显示文本。
* `tip` 节点的提示文字。
* `value` 节点的值，不可见。

## 方法

* `decrease(decrement = 1)` 如果节点的`tip`是数字，则将数字递减`decrement`。与`increase(increment)`方法对应。
* `if(exp)` 接受一个函数或布尔表达式或表达式字符串。检查是否满足指定的条件，满足返回自己，不满足返回`null`。`exp`如果是一个函数，需返回布尔值，参数传递当前节点；`exp`如果是一个字符串，则先进行运算再判断，字符串中支持使用`this`关键字，指向当前节点；也可以是布尔值或布尔表达式。
    ```javascript
    $s('#TreeView1').selectedNode?.if(node => node.name == 'Node1')?.increase();
    $s('#TreeView1').selectedNode?.if(function(){ return this.name == 'Node1'; })?.increase();
    $s('#TreeView1').selectedNode?.if("this.name == 'Node1'")?.increase();
    ```
* `increase(increment = 1)` 如果节点的`tip`是数字，则将数字递增`increment`。与`decrease(decrement)`方法对应。
* `reload()` 重新加载节点的数据，即节点的子节点。
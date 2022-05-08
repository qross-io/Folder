
# Javascript 用数组实现队列和栈

在 Javascript 中，集合数组结构非常少，常用的只有数组、对象、Set 和 Map，跟 Java 根本没法比。如果我们要实现队列 Queue 和栈 Stack 的功能怎么办？Javascript 数组提供了相关的方法用于实现需要的功能。

## 先进先出的队列

有两个方向，一个是尾部进元素头部出元素，一个是头部进元素尾部出元素。

尾部进头部出，可以用`push`方法添加元素和`shift`方法移除元素。

```javascript
let array = [1, 2, 3];
array.push(4); // array = [1, 2, 3, 4];
let e = array.shift(); //e = 1, a = [2, 3, 4];
```

`push`方法可以在数组尾部添加一个或多个元素，`shift`方法从数组头部移除元素，返回被移除的元素。

头部进尾部出，可以用`unshift`添加元素和`pop`方法移除元素。

```javascript
let array = [1, 2, 3];
array.unshift(4); // array = [4, 1, 2, 3];
let e = array.pop(); //e = 3, a = [4, 1, 2];
```

`unshift`方法可以在数组头部添加一个或多个元素，`pop`方法可以从数组属性移除元素并返回移除的元素。

## 后进先出的栈

还是上一节中的四个方法，也同样有两个方向，一个是尾部进尾部出，一个是头部进头部出。

尾部进尾部出，用`push`和`pop`配合。

```javascript
let array = [1, 2, 3];
array.push(4); // array = [1, 2, 3, 4];
let e = array.pop(); //e = 4, a = [1, 2, 3];
```

头部进头部出，用`unshift`和`shift`配合。

```javascript
let array = [1, 2, 3];
array.unshift(4); // array = [4, 1, 2, 3];
let e = array.unshift(); //e = 4, a = [1, 2, 3];
```

（本文完）
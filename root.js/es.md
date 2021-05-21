# ECMAScript 新版本特性

## ES 10

* `Array.prototype.flat()`
* `Array.prototype.flatMap()`
* `Object.fromEntries()` 是 `Object.entries()` 逆操作。
* `String.prototype.trimStart()`
* `String.prototype.trimEnd()`
* `String.prototype.matchAll()` 取出字符串中的所有匹配。
* `try...catch..` 现在 catch 参数`e`为可选项。
* 新的数据类型大整数 BigInt，如 `let x = 123n`。
* `Symbol.prototype.description` 
* `Function.prototype.toString()`


## ES 9

* 异步迭代器 `for await (let item of collection) { ... }`
* Object Rest Spread，对象属性复制。
    ```javascript
    const input = {
    a: 1,
    b: 2,
    c: 1
    }
    const output = {
    ...input,
    c: 3
    }
    console.log(output) // {a: 1, b: 2, c: 3}
    ```
* `Promise.prototype.finally`
* 正则表达式新特性：dotAll、命名捕获组、后行断言、Unicode 转义


## ES 8

* 异步函数 `asyn/await function () { ... }` 返回一个 Promise 对象。
* `Object.entries()` 将一个对象中可枚举属性的键名和键值按照二维数组的方式返回。
    ```javascript
    Object.entries({ one: 1, two: 2 })    //[['one', 1], ['two', 2]]
    var map2 = new Map(Object.entries(obj));
    ```
* `Object.keys()` 只返回自己的键值对中键的值。
* `Object.values()` 只返回自己的键值对中属性的值。
* `String.prototype.padStart(targetLength, padding)` 用字符 padding 填充字符串左侧至 targetLength 长度。
* `String.prototype.padEnd(targetLength, padding)` 用字符 padding 填充字符串右侧至 targetLength 长度。
* `Object.getOwnPropertyDescriptors()` 该方法会返回目标对象中所有属性的属性描述符，该属性必须是对象自己定义的，不能是从原型链继承来的。
* 函数参数支持尾部逗号。
* 装饰器`@decorator`，可以用到类和方法上，不能用于函数上，可以参，可以使用多个。
    ```javascript
    @speak("hello world")
    class Person { }
    function speak(text) {
        return function(target) {
            target.say = text;
        }
    }
    console.log(Person.say)  //'hello world'

    class Person {
        constructor() {}
        @speak
        say() {
            //...
        }
    }
    function speak(target, key, descriptor) {
        console.log(target);
        console.log(key);
        console.log(descriptor);
        descriptor.value = function() {
            //...
        }
    }

    let person = new Person()
    person.say()
    ```
    `target` 类的原型对象，上例是 Person.prototype，`key` 所要修饰的类名，`descriptor` 该属性的描述对象

## ES 7

* `Array.prototype.includes()` 判断数组中是否包含某个值。
* 求幂运算符`**`，例如`3 ** 2`得到`9`，与`Math.pow`等价。
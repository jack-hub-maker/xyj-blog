# ES8 新特性

## 一、Object values

之前我们可以通过 Object.keys 获取一个对象所有的 key，在 ES8 中提供了 Object.values 来获取所有的 value 值：

```js
const obj = {
  name: "tao",
  age: 18,
};

console.log(Object.values(obj)); // [ 'tao', 18 ]
console.log(Object.values(["zs", "ls", "ww"])); // [ 'zs', 'ls', 'ww' ]
console.log(Object.values("abc")); // [ 'a', 'b', 'c' ]
```

## 二、Object entries

通过 Object.entries 可以获取到一个数组，数组中会存放可枚举属性的键值对数组。

```js
const obj = {
  name: "tao",
  age: 18,
};

console.log(Object.entries(obj)); // [ [ 'name', 'tao' ], [ 'age', 18 ] ]
console.log(Object.entries(["zs", "ls", "ww"])); // [ [ '0', 'zs' ], [ '1', 'ls' ], [ '2', 'ww' ] ]
console.log(Object.entries("abc")); // [ [ '0', 'a' ], [ '1', 'b' ], [ '2', 'c' ] ]
```

## 三、String Padding

某些字符串我们需要对其进行前后的填充，来实现某种格式化效果，ES8 中增加了 padStart 和 padEnd 方法，分
别是对字符串的首尾进行填充的。

```js
const message = "Hello World";
console.log(message.padStart(15, "*").padEnd(20, "-")); // ****Hello World-----
```

我们简单具一个应用场景：比如需要对身份证、银行卡的前面位数进行隐藏：

```js
const cardNumber = "510103196502083435";
const lastCartNumber = cardNumber.slice(-4);
const newCardNumber = lastCartNumber.padStart(18, "*");
console.log(newCardNumber); // **************3435
```

## 四、Trailing Commas

在 ES8 中，我们允许在函数定义和调用时多加一个逗号：

```js
function foo(x, y,) {
  console.log(x, y);  // 10 20
}
foo(10, 20,)
```

!>这算是从其它编程语言开发者转到 JavaScript 的一种习惯吧，喜欢写完一个东西就在后面加上一个逗号，方便后续扩展

## 五、Object Descriptors

ES8中增加了另一个对对象的操作是 Object.getOwnPropertyDescriptors ，这个在面向对象的时候已经讲过了，这里不再重
复。

后续补充
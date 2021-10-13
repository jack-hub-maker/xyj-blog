# ES7 新特性

!>讲新特性的话，我主要列举一些**常用**的新特性，没有说到的新特性详情去官网看[点击跳转]()

## 一、Array Includes

在 ES7 之前，如果我们想判断一个数组中是否包含某个元素，需要通过 indexOf 获取结果，并且判断是否为 -1。

```js
const names = ["zs", "ls", "ww", "zl", NaN];

if (names.indexOf("zl") !== -1) {
  console.log("zl在数组中");
} else {
  console.log("zl不在数组中");
}
// zl在数组中
```

但是 indexOf 是判断不了 NaN 的(虽然开发中我们很少会看到 NaN 在一个数组里)

```js
const names = ["zs", "ls", "ww", "zl", NaN];

if (names.indexOf(NaN) !== -1) {
  console.log("NaN在数组中");
} else {
  console.log("NaN不在数组中");
}
// NaN不在数组中
```

在 ES7 中，我们可以通过 includes 来判断一个数组中是否包含一个指定的元素，根据情况，如果包含则返回 true，
否则返回 false。

```js
const names = ["zs", "ls", "ww", "zl", NaN];

if (names.includes(NaN)) {
  console.log("NaN在数组中");
} else {
  console.log("NaN不在数组中");
}
// NaN在数组中
```

## 二、指数运算

在 ES7 之前，计算数字的乘方需要通过 Math.pow 方法来完成。

```js
// 3的3次方
console.log(Math.pow(3, 3)); // 27
```

在 ES7 中，增加了 \*\* 运算符，可以对数字来计算乘方。

```js
// 3的3次方
console.log(3 ** 3); // 27
```

!>从 ES6 之后新特性就不是很多，每年增加一点（ES6 算是一次大更新）

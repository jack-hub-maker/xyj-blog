## 理解组合函数

组合（Compose）函数是在 JavaScript 开发过程中一种对函数的使用技巧、模式：

- 比如我们现在需要对`某一个数据`进行`函数的调用`，执行`两个函数fn1和fn2`，这`两个函数是依次执行`的；
- 那么如果每次我们都需要`进行两个函数的调用`，`操作上就会显得重复`；
- 那么`是否可以将这两个函数组合起来，自动依次调用`呢？
- 这个过程就是`对函数的组合`，我们称之为 `组合函数（Compose Function）`；

```js
function double(num) {
  return num * 2;
}

function square(num) {
  return num ** 2;
}

var count = 10;
var result = square(double(count));
console.log(result);

// 组合函数
function compose(f1, f2) {
  return function (x) {
    return f2(f1(x));
  };
}

var calcFn = compose(double, square);
console.log(calcFn(10));
```

## 实现组合函数

刚才我们实现的 compose 函数比较简单，我们需要考虑更加复杂的情况：比如传入了更多的函数，在调用
compose 函数时，传入了更多的参数：

```js
function gtCompose(...fns) {
  // 遍历所有参数，如果传入的参数不是函数，那么直接报错
  for (var i = 0; i < fns.length; i++) {
    var fn = fns[i];
    if (typeof fn !== "function") {
      throw new TypeError("The argument should be a function type");
    }
  }

  // 取出所有函数的第一次调用
  return function (...args) {
    // 先取出第一次的结果
    var index = 0;
    var result = fns.length ? fns[index].call(this, ...args) : args;
    while (++index < fns.length) {
      result = fns[index].call(this, result);
    }
    return result;
  };
}

function double(count) {
  return count * 2;
}

function square(count) {
  return count ** 2;
}

var result = gtCompose(double, square);
console.log(result(10));
```

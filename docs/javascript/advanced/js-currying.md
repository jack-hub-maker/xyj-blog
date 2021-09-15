## JavaScript 柯里化

柯里化也是属于函数式编程里面一个非常重要的概念。

我们先来看一下维基百科的解释：

- 在计算机科学中，柯里化（英语：Currying），又译为卡瑞化或加里化；
- 是把接收`多个参数的函数`，变成`接受一个单一参数`（最初函数的第一个参数）的函数，并且`返回接受余下的参数`，而且`返回结果的新函数`的技术；
- 柯里化声称 “`如果你固定某些参数，你将得到接受余下参数的一个函数`”；

维基百科的结束非常的抽象，我们这里做一个总结：

- 只`传递给函数一部分参数来调用它`，让`它返回一个函数去处理剩余的参数`；
- `这个过程就称之为柯里化`；

## 柯里化的结构

我们来看下面的图

```js
// 未柯里化的函数
function foo(x, y, z) {
  return x + y + z;
}
console.log(foo(10, 20, 30));

// 柯里化的函数
function bar(x) {
  return function (y) {
    return function (z) {
      return x + y + z;
    };
  };
}

console.log(bar(10)(20)(30));

// 我们还可以使用箭头函数
var baz = (x) => {
  return (y) => {
    return (z) => {
      return x + y + z;
    };
  };
};
console.log(bar(10)(20)(30));

// 箭头函数也可以进行优化（诧异的可以看看前面this的文章）
var info = (x) => (y) => (z) => x + y + z;
console.log(info(10)(20)(30));
```

## 让函数的职责单一

那么为什么需要有柯里化呢？

- 在函数式编程中，我们其实往往希望一个函数处理的问题尽可能的单一，而不是将一大堆的处理过程交给一个
  函数来处理；
- 那么我们是否就可以将每次传入的参数在单一的函数中进行处理，处理完后在下一个函数中再使用处理后的结
  果；

比如上面的案例我们进行一个修改：传入的函数需要分别被进行如下处理

- 第一个参数+2
- 第二个参数\*2
- 第三个参数\*\*2
  然后得到三个参数相加的值

```js
// 未柯里化的函数
function foo(x, y, z) {
  x = x + 2;
  y = y * 2;
  z = z * 2 * 2;
  return x + y + z;
}

console.log(foo(2, 2, 2));

// 柯里化后的函数

function bar(x) {
  x = x + 2;
  return function (y) {
    y = y * 2;
    return function (z) {
      z = z * 2 * 2;
      return x + y + z;
    };
  };
}

console.log(bar(2)(2)(2));
```

当然可能你会觉得感觉没什么区别啊

我们这里只是逻辑非常简单，如果我们对每一个参数处理的代码都有 10 行，我们全部放在一个函数中进行处理的话，代码结构就不是很清晰

但是如果我们把每一个参数单独放在一个函数中进行处理的话，代码结构会变得比较清晰

## 柯里化的复用

另外一个使用柯里化的场景是可以帮助我们可以复用参数逻辑：

```js
// foo函数中的num始终与count相加
function foo(num, count) {
  return count + num;
}

console.log(foo(3, 7));
console.log(foo(3, 7));
console.log(foo(3, 7));

// 柯里化
var bar = (num) => (count) => count + num;
var result = bar(3);
console.log(result(7));
console.log(result(7));
console.log(result(7));
```

当然这个简单的场景可能看不出有很强的复用性

接下来看这个例子

```js
function foo(date, type, message) {
  console.log(
    `[${date.getHours()}]:${date.getMinutes()}] [${type}] [${message}]`
  );
}

foo(new Date(), "DEBUG", "修复bug");
foo(new Date(), "FEATURE", "新功能");

// 柯里化
// 我们就可以对其进行定制化
var bar = (date) => (type) => (message) =>
  console.log(
    `[${date.getHours()}]:${date.getMinutes()}] [${type}] [${message}]`
  );

var logNow = bar(new Date());
logNow("DEBUG")("修复bug");
logNow("FEATURE")("新功能");

var logNowDebug = bar(new Date(), "DEBUG");
logNowDebug("轮播图bug");
logNowDebug("按钮bug");
```

## 自动柯里化函数

目前我们有将多个普通的函数，转成柯里化函数：

```js
function add(x, y, z) {
  return x + y + z;
}

function gtCurrying(fn) {
  return function curried(...args) {
    // 判断当前已经接收的参数的个数，可以参数本身需要接受的参数是否已经一致
    // 1.当已经传入的参数大于等于需要的参数时，就执行函数
    if (args.length >= fn.length) {
      return fn.apply(this, args);
    } else {
      // 没有达到个数时，需要返回一个新的函数，继续来接收参数
      return function (...args2) {
        // 接收到参数后，需要递归调用curried来检查函数的个数是否达到
        curried.apply(this, [...args, ...args2]);
      };
    }
  };
}

var result = gtCurrying(add);

console.log(result(10, 20, 30));
console.log(result(10, 20)(30));
console.log(result(10)(20)(30));
```

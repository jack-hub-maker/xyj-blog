# 函数式编程
## 一、纯函数
### 1.1 理解 JavaScript 纯函数

函数式编程中有一个非常重要的概念叫纯函数，JavaScript 符合函数式编程的范式，所以也有纯函数的概念；

- 在 react 开发中纯函数是被多次提及的；
- 比如 react 中组件就被要求像是一个纯函数（为什么是像，因为还有 class 组件），redux 中有一个 reducer 的概念，也
  是要求必须是一个纯函数；
- 所以掌握纯函数对于理解很多框架的设计是非常有帮助的；

纯函数的维基百科定义：

- 在程序设计中，若一个函数`符合以下条件`，那么这个函数被称为纯函数：
- 此函数`在相同的输入值时`，需`产生相同的输出`。
- 函数的`输出和输入值以外的其他隐藏信息或状态无关`，也和`由I/O设备产生的外部输出`无关。
- 该函数`不能有语义上可观察的函数副作用`，诸如“`触发事件`”，`使输出设备输出，或更改输出值以外物件的内容`等。

当然上面的定义会过于的晦涩，所以我简单总结一下：

- `确定的输入，一定会产生确定的输出`；
- `函数在执行过程中，不能产生副作用`;

### 1.2 副作用的理解

那么这里又有一个概念，叫做副作用，什么又是副作用呢？

- 副作用（side effect）其实本身是医学的一个概念，比如我们经常说吃什么药本来是为了治病，可能会产生一
  些其他的副作用；
- 在计算机科学中，也引用了副作用的概念，表示`在执行一个函数`时，除了`返回函数值`之外，还对`调用函数产生了附加的影响`，比如`修改了全局变量`，`修改参数或者改变外部的存储`；

纯函数在执行的过程中就是不能产生这样的副作用：副作用往往是产生`bug的 “温床”`。

!>其实无论是 vue3 和 react 越来越趋向于函数式编程，无论是 vue 中的单向数据流还是 react 中的纯函数其实都是一个概念，所以理解学习纯函数是非常重要的，但是我们不能保证我们所编写的函数都是纯函数，比如有些时候我们的初衷就是想要改变全局的一个东西，这其实是无法避免的，所以我们应该尽量编写纯函数

### 1.3 纯函数的案例

我们来看一个对数组操作的两个函数：

- slice：slice 截取数组时不会对原数组进行任何操作,而是生成一个新的数组；
- splice：splice 截取数组, 会返回一个新的数组, 也会对原数组进行修改；
- slice 就是一个纯函数，不会修改传入的参数；

所以当我们想有这种的需求的时候，有这两种方案，我们应该尽量使用 slice

```js
var names = ["tao", "xyj", "zm", "ymy"];

// slice截取，会返回一个新的数组，但是不会对原数组进行修改
var newNames = names.slice(0, 2);
console.log(newNames);
console.log(names);

// splice截取数组，会返回一个新的数组，也会对原数组进行修改
var newNames2 = names.splice(0, 2);
console.log(newNames2);
console.log(names);
```

下面我们来看几个案例

```js
// 这个很明显就是一个纯函数
function sum(num1, num2) {
  return num1 + num2;
}
```

```js
// 这个显然不是纯函数
// 相同的输入没有产生相同的输出
var age = 5;

function add(num) {
  return age + num;
}

console.log(add(5));
age = 10;
console.log(add(5));
```

```js
// 这个函数不是一个纯函数
// 虽然相同的输入，会有相同的输出
// 但是函数修改了参数，产生了副作用
var info = {
  name: "tao",
  age: 19,
};

function foo(info) {
  info.name = "xyj";
}

foo(info);
```

```js
// 这个其实在社区还有很多争论的
// 函数的输出与外部的环境无关
// console.log是不是也算输出
// 输出了外部的age
// 这个问题我的看法是：从严格的角度来说这个函数不是一个纯函数
// 但是如果是我的话，我会觉得这个是一个纯函数，我只是打印了外部的age看一下，并没有产生任何的副作用
var age = 19;
function foo() {
  console.log(age);
}
```

### 1.4 纯函数的优势

为什么纯函数在函数式编程中非常重要呢？

- 因为你可以`安心的编写`和`安心的使用`；
- 你在`写的时候`保证了函数的纯度，只是`单纯实现自己的业务逻辑`即可，`不需要关心传入的内容`是如何获得的或
  者`依赖其他的外部变量`是否已经发生了修改；
- 你在`用的时候`，你确定`你的输入内容不会被任意篡改`，并且`自己确定的输入`，一定会`有确定的输出`；

React 官方就要求我们无论是函数还是 class 声明一个组件，这个组件都必须像纯函数一样，保护它们的 props 不被修
改：

## 二、柯里化

### 2.1 认识柯里化

柯里化也是属于函数式编程里面一个非常重要的概念。

我们先来看一下维基百科的解释：

- 在计算机科学中，柯里化（英语：Currying），又译为卡瑞化或加里化；
- 是把接收`多个参数的函数`，变成`接受一个单一参数`（最初函数的第一个参数）的函数，并且`返回接受余下的参数`，而且`返回结果的新函数`的技术；
- 柯里化声称 “`如果你固定某些参数，你将得到接受余下参数的一个函数`”；

维基百科的结束非常的抽象，我们这里做一个总结：

- 只`传递给函数一部分参数来调用它`，让`它返回一个函数去处理剩余的参数`；
- `这个过程就称之为柯里化`；

### 2.2 柯里化的结构

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

### 2.3 让函数的职责单一

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

### 2.4 柯里化的复用

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

### 2.5 自动柯里化函数

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

## 三、组合函数
### 3.1 理解组合函数

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

### 3.2 实现组合函数

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

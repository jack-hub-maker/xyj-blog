# ES6+

## ES7

### 一、Array Includes

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

在 ES7 中，我们可以通过 [includes](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/includes) 来判断一个数组中是否包含一个指定的元素，根据情况，如果包含则返回 true，
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

### 二、指数运算符

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

## ES8

### 一、Object values

之前我们可以通过 Object.keys 获取一个对象所有的 key，在 ES8 中提供了 [Object.values](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/values) 来获取所有的 value 值：

```js
const obj = {
  name: "tao",
  age: 18,
};

console.log(Object.values(obj)); // [ 'tao', 18 ]
console.log(Object.values(["zs", "ls", "ww"])); // [ 'zs', 'ls', 'ww' ]
console.log(Object.values("abc")); // [ 'a', 'b', 'c' ]
```

### 二、Object entries

通过 [Object.entries](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/entries) 可以获取到一个数组，数组中会存放可枚举属性的键值对数组。

```js
const obj = {
  name: "tao",
  age: 18,
};

console.log(Object.entries(obj)); // [ [ 'name', 'tao' ], [ 'age', 18 ] ]
console.log(Object.entries(["zs", "ls", "ww"])); // [ [ '0', 'zs' ], [ '1', 'ls' ], [ '2', 'ww' ] ]
console.log(Object.entries("abc")); // [ [ '0', 'a' ], [ '1', 'b' ], [ '2', 'c' ] ]
```

### 三、String Padding

某些字符串我们需要对其进行前后的填充，来实现某种格式化效果，ES8 中增加了 [padStart](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/padStart) 和 [padEnd](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/padEnd) 方法，分
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

### 四、Trailing Commas

在 ES8 中，我们允许在函数定义和调用时多加一个逗号：

```js
function foo(x, y) {
  console.log(x, y); // 10 20
}
foo(10, 20);
```

?>这算是从其它编程语言开发者转到 JavaScript 的一种习惯吧，喜欢写完一个东西就在后面加上一个逗号，方便后续扩展

### 五、Object Descriptors

[Object.getOwnPropertyDescriptors()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptors)函数用来获取一个对象的所有自身属性的描述符,如果没有任何自身属性，则返回空对象。

### 六、async/await

#### 6.1 异步函数 async function

async 关键字用于声明一个异步函数：

- async 是 asynchronous 单词的缩写，异步、非同步；
- sync 是 synchronous 单词的缩写，同步、同时；

async 异步函数可以有很多中写法：

```js
async function foo1() {}

const foo2 = async function () {};

const foo3 = async () => {};

class Person {
  async foo() {}
}
```

#### 6.2 异步函数的执行流程

异步函数的内部代码执行过程和普通的函数是一致的，默认情况下也是会被同步执行。

```js
async function foo() {
  console.log("start");
  console.log(1);
  console.log(2);
  console.log(3);
  console.log("end");
}

foo();
console.log("abc");

/**
start
1
2
3
end
abc
 */
```

异步函数有返回值时，和普通函数会有区别：

情况一：异步函数也可以有返回值，但是异步函数的返回值会被包裹到 Promise.resolve 中；

```js
async function foo() {
  console.log("start");
  console.log(1);
  console.log(2);
  console.log(3);
  console.log("end");
}

// 默认没有返回值，那就是return undefined
foo().then((res) => {
  console.log(res);
});
console.log("abc");

/**
start
1
2
3
end
abc
undefined
 */
```

情况二：如果我们的异步函数的返回值是 Promise，Promise.resolve 的状态会由 Promise 决定；

```js
async function foo() {
  console.log("start");
  console.log(1);
  console.log(2);
  console.log(3);
  console.log("end");
  return new Promise((resolve, reject) => {
    resolve("aaa");
  });
}

foo().then((res) => {
  console.log(res);
});
console.log("abc");

/**
start
1
2
3
end
abc
aaa
 */
```

情况三：如果我们的异步函数的返回值是一个对象并且实现了 thenable，那么会由对象的 then 方法来决定；

```js
async function foo() {
  console.log("start");
  console.log(1);
  console.log(2);
  console.log(3);
  console.log("end");
  return {
    then(resolve, reject) {
      resolve("bbb");
    },
  };
}

foo().then((res) => {
  console.log(res);
});
console.log("abc");

/**
start
1
2
3
end
abc
bbb
 */
```

如果我们在 async 中抛出了异常，那么程序它并不会像普通函数一样报错，而是会作为 Promise 的 reject 来传递；

```js
async function foo() {
  console.log("start");
  console.log(1);
  console.log(2);
  console.log(3);
  console.log("end");
  throw new Error("抛出异常");
}

foo().catch((err) => {
  console.log("捕获到异常");
});
console.log("abc");

/**
start
1
2
3
end
abc
捕获到异常
 */
```

#### 6.3 await 关键字

**async 函数另外一个特殊之处**就是可以在它内部使用**await 关键字**，而**普通函数中是不可以**的。

await 关键字有什么特点呢？

- 通常使用 await 是后面会`跟上一个表达式`，这个表达式会`返回一个Promise`；
- 那么 await 会`等到Promise的状态变成fulfilled状态`，之后`继续执行异步函数`；

```js
function request() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve("aaa");
    }, 1000);
  });
}

async function foo() {
  const res = await request();
  console.log(1, res);
  console.log(2);
  console.log(3);
}

foo();

console.log("abc");

/**
abc
1 aaa
2
3
 */
```

如果 await 后面是一个普通的值，那么会直接返回这个值；

```js
async function foo() {
  // 其实我觉得内部它这里会把bbb给转为生成一个新的promise的resolve的值然后返回给res
  const res = await "bbb";
  console.log(1, res);
  console.log(2);
  console.log(3);
}

foo();

console.log("abc");

/**
abc
1 bbb
2
3
 */
```

如果 await 后面是一个 thenable 的对象，那么会根据对象的 then 方法调用来决定后续的值；

```js
async function foo() {
  const res = await {
    then(resolve, reject) {
      resolve("ccc");
    },
  };
  console.log(1, res);
  console.log(2);
  console.log(3);
}

foo();

console.log("abc");

/**
abc
1 ccc
2
3
 */
```

如果 await 后面的表达式，返回的 Promise 是 reject 的状态，那么会将这个 reject 结果直接作为函数的 Promise 的
reject 值；

```js
function request() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      reject("fff");
    }, 1000);
  });
}

async function foo() {
  const res = await request();
  console.log(1, res);
  console.log(2);
  console.log(3);
}

foo().catch((err) => {
  console.log("err", err);
});

console.log("abc");

/**
abc
err fff
 */
```

诶你可能会有些疑惑，后面的 1，2，3 为什么没有打印

其实你可以理解为上面 res 后面的三句打印相对于是在 then 函数里的打印的

之前我们说过 async/await 其实是 generator 和 promise 实现的一个自动执行的语法糖

都在 then 里面，那么上面的代码 Promise 是 reject 就不会来到 then 方法里，只会来到 catch 里面，所以后面的打印是不会执行的

## ES9

### 一、Async iterators

await 可以和 for...of 循环一起使用，以串行的方式运行异步操作

```js
async function process(array) {
  for await (let i of array) {
    console.log(i);
  }
}
```

### 二、Object spread operators

可以对对象使用展开语法

```js
var obj1 = { foo: "bar", x: 42 };
var obj2 = { foo: "baz", y: 13 };

var clonedObj = { ...obj1 };
// 克隆后的对象: { foo: "bar", x: 42 }

var mergedObj = { ...obj1, ...obj2 };
// 合并后的对象: { foo: "baz", x: 42, y: 13 }
```

### 三、Promise finally

[finally](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/finally)表示无论 Promise 对象无论变成 fulfilled 还是 reject 状态，最终都会 被执行的代码。

finally 方法是不接收参数的，因为无论前面是 fulfilled 状态，还是 reject 状态，它都会执行。

```js
const p = new Promise((resolve, reject) => {
  reject("aaa");
});

p.then((res) => {
  console.log("res", res);
})
  .catch((err) => {
    console.log("err", err);
  })
  .finally(() => {
    console.log("finally");
  });

// err aaa
// finally
```

## ES10

### 一、flat flatMap

[flat()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/flat) 数组扁平化

```js
// 三维数组转为一维数组
const arr = [
  1,
  2,
  [321, 32],
  [
    [3213, 321],
    [3213, 3213],
  ],
];
// 降维两次
const newArr = arr.flat(2);
console.log(newArr); // [1, 2, 321, 32, 3213, 321, 3213, 3213]
```

传入`Infinity`，可展开任意深度的嵌套数组

```js
const arr = [1, [2, 3, [[4, 5, [[[6]]]]]]];
const newArr = arr.flat(Infinity);
console.log(newArr); // [ 1, 2, 3, 4, 5, 6 ]
```

[flatMap()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/flatMap) 方法首先使用映射函数映射每个元素，然后将结果压缩成一个新数组。

- 注意一：flatMap 是先进行 map 操作，再做 flat 的操作；
- 注意二：flatMap 中的 flat 相当于深度为 1；

```js
const message = [
  "Hello World",
  "Hello JavaScript",
  "Hello Vue",
  "Hello React",
];
const newMessage = message.flatMap((item) => {
  return item.split(" ");
});
console.log(newMessage); // ['Hello', 'World', 'Hello', 'JavaScript', 'Hello', 'Vue', 'Hello', 'React']
```

### 二、Object fromEntries

[Object.fromEntries()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/fromEntries) 方法把键值对列表转换为一个对象。

比如我们前端可能有些情况需要对 query 进行处理

```js
const paramsString = "name=tao&age=18&height=1.88";
const searchParams = new URLSearchParams(paramsString);
const searchObj = {};
for (const param of searchParams) {
  searchObj[param[0]] = param[1];
}
console.log(searchObj); // { name: 'tao', age: '18', height: '1.88' }
```

使用 fromEntries 就会相对比较简洁

```js
const paramsString = "name=tao&age=18&height=1.88";
const searchParams = new URLSearchParams(paramsString);
const searchObj = Object.fromEntries(searchParams);
console.log(searchObj); // { name: 'tao', age: '18', height: '1.88' }
```

### 三、trimStart trimEnd

去除一个字符串首尾的空格，我们可以通过 trim 方法，如果单独去除前面或者后面呢？

ES10 中给我们提供了 [trimStart](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/trimStart) 和 [trimEnd](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/trimEnd)

```js
const message = "      Hello World       ";
console.log(message.trim()); // Hello World
console.log(message.trimStart()); // Hello World_____
console.log(message.trimEnd()); //____ Hello World
```

### 四、Symbol description

[description](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol/description)返回 Symbol 的属性描述值(只读)

```js
console.log(Symbol("xyj").description); // xyj
```

### 五、Optional catch binding

在以往的版本中，try-catch 里 catch 后面必须带异常参数，例如：

```js
try {
} catch (error) {}
```

但是在 ES10 之后，这个参数变成了可选，如果用不到，我们可以不用传，例如：

```js
try {
} catch {}
```

### 六、String.prototype.matchAll

[matchAll()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/matchAll) 为所有匹配的匹配对象返回一个迭代器

## ES11

### 一、BigInt

在早期的 JavaScript 中，我们不能正确的表示过大的数字：

大于 MAX_SAFE_INTEGER 的数值，表示的可能是不正确的。（MAX_SAFE_INTEGER 不是最大的数，而是如果比 MAX_SAFE_INTEGER 还大的话计算起来可能不是准确的）（**不安全**）

```js
const maxInt = Number.MAX_SAFE_INTEGER;
console.log(maxInt); // 9007199254740991
console.log(maxInt + 1); // 9007199254740992
console.log(maxInt + 2); // 9007199254740992
```

那么 ES11 中，引入了新的数据类型 [BigInt](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/BigInt)，用于表示大的整数：
BitInt 的表示方法是在数值的后面加上 n

```js
const bigInt = 90071992547409910n;
console.log(bigInt); // 90071992547409910n
// console.log(bigInt + 10); // TypeError: Cannot mix BigInt and other types, use explicit conversions
// 其实在很多语言之间都会有一个隐式转换的概念，有些语言就没有，比如10和3.14在一些语言里这两个是无法相加的，需要把10转换为浮点性，然后两者再进行相加
// 这是因为两个类型是不同，bigInt是BigInt类型，而10是Number类型，两者是无法相加的
console.log(bigInt + 10n); // 90071992547409920n
```

### 二、Nullish Coalescing Operator

在以前如果我们想对一个元素判断是否有值，然后处理对应的操作

```js
const foo = null;
const message = foo || "default value";
console.log(message); // default value
```

但是 || 运算符还是有一定的弊端，判断是否有值，它会做一个隐式转换(转换为布尔值)，比如空字符串，0 它都会判定没有值

```js
const foo = 0;
const message = foo || "default value";
console.log(message); // default value
```

ES11，[Nullish Coalescing Operator](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator) 增加了空值合并操作符(它会明确判断前者是否有值)

```js
const foo = 0;
const message = foo ?? "default value";
console.log(message); // 0
```

### 三、Optional Chaining（可选链）

我们先来看一个案例

```js
const obj = {
  name: "tao",
  friend: {
    name: "xyj",
    girlfriend: {
      name: "zm",
    },
  },
};

console.log(obj.friend.girlfriend.name); // zm

// 但是有一天我们删除了obj.friends，那么上面的代码就会报错

// 如果按照以前的做法的话，我们可以会写一个if判断
if (obj.friend && obj.friend.girlfriend) {
  console.log(obj.friend.girlfriend.name); // zm
}
console.log("其它逻辑代码"); // 其它逻辑代码
```

但是如果代码中有很多这样类似的逻辑，那么我们会写很多的 if（整体来说很冗余）

[可选链](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Optional_chaining)也是 ES11 中新增一个特性，主要作用是让我们的代码在进行 null 和 undefined 判断时更加清晰和简洁：

```js
const obj = {
  name: "tao",
  friend: {
    name: "xyj",
    girlfriend: {
      name: "zm",
    },
  },
};
console.log(obj.friend?.girlfriend?.name); // zm

console.log("其它逻辑代码"); // 其它逻辑代码
```

### 四、Global This

在之前我们希望获取 JavaScript 环境的全局对象，不同的环境获取的方式是不一样的

- 比如在浏览器中可以通过 this、window 来获取；
- 比如在 Node 中我们需要通过 global 来获取；

但是有些情况代码可能会在浏览器执行或者在 Node 环境下执行（我们无法判断的话可能会 if-else 来进行判断）

那么在 ES11 中对获取全局对象进行了统一的规范：[globalThis](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/globalThis)

```js
// 在浏览器环境下执行打印的window对象，在Node环境下执行打印的是global对象
console.log(globalThis);
```

### 五、for..in 标准化

在 ES11 之前，虽然很多浏览器支持 for...in 来遍历对象类型，但是并没有被 ECMA 标准化。（有些浏览器获取的 item 是 value）

在 ES11 中，对其进行了标准化，for...in 是用于遍历对象的 key 的：

```js
const obj = {
  name: "tao",
  age: 18,
};

for (const item in obj) {
  console.log(item); // name age
}
```

### 六、import meta

[import.meta](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/import.meta)是一个给 JavaScript 模块暴露特定上下文的元数据属性的对象。它包含了这个模块的信息，比如说这个模块的 URL。

```js
console.log(import.meta); // { url: "file:///home/user/my-module.mjs" }
```

### 七、Promise.allSettled

all 方法有一个缺陷：当有其中一个 Promise 变成 reject 状态时，新 Promise 就会立即变成对应的 reject 状态。

- 那么对于 resolved 的，以及依然处于 pending 状态的 Promise，我们是获取不到对应的结果的；
  在 ES11（ES2020）中，添加了新的 API Promise.allSettled：

- 该方法会在所有的 Promise 都有结果（settled），无论是 fulfilled，还是 reject 时，才会有最终的状态；
- 并且这个 Promise 的结果一定是 fulfilled 的；

```js
const p1 = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject("aaa");
  }, 1000);
});
const p2 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("bbb");
  }, 2000);
});
const p3 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("ccc");
  }, 3000);
});

Promise.allSettled([p3, p2, p1])
  .then((res) => {
    console.log("res", res);
  })
  .catch((err) => {
    console.log("err", err);
  });

/**
 * res [
  { status: 'fulfilled', value: 'ccc' },
  { status: 'fulfilled', value: 'bbb' },
  { status: 'rejected', reason: 'aaa' }
]
 */
```

allSettled 的结果是一个数组，数组中存放着每一个 Promise 的结果，并且是对应一个对象的；

这个对象中包含 status 状态，以及对应的 value 值；

### 八、import()

通过 import 加载一个模块，是不可以在其放到逻辑代码中的，比如：

它包含了这个模块的信息，比如说这个模块的 URL；

```js
if (true) {
  import foo from "./foo.js";
}
// Uncaught SyntaxError: Unexpected identifier
```

为什么会出现这个情况呢？

- 这是因为 ES Module 在被 JS 引擎解析时，就必须知道它的依赖关系；
- 由于这个时候 js 代码没有任何的运行，所以无法在进行类似于 if 判断中根据代码的执行情况；
  但是某些情况下，我们确确实实希望动态的来加载某一个模块：

- 如果根据不懂的条件，动态来选择加载模块的路径；
- 这个时候我们需要使用 [import()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/import#%E5%8A%A8%E6%80%81import) 函数来动态加载；

```js
if (true) {
  // import函数返回的是promise
  import("./foo.js").then((res) => {
    console.log(res);
  });
}
```

## ES12

### 一、FinalizationRegistry

当我们把一个对象指向 null 的时候，我们知道这个对象会被 GC 回收掉的，但是想知道这个对象什么时候会被回收掉

[FinalizationRegistry](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/FinalizationRegistry) 对象可以让你在对象被垃圾回收时请求一个回调。

- FinalizationRegistry 提供了这样的一种方法：当一个在注册表中注册的对象被回收时，请求在某个时间点上调
  用一个清理回调。（清理回调有时被称为 finalizer ）;
- 你可以通过调用 register 方法，注册任何你想要清理回调的对象，传入该对象和所含的值;

```js
const finalizationRegistry = new FinalizationRegistry((value) => {
  console.log(`注册在finalizationRegistry上的${value}对象被GC回收了`);
});

let obj = {
  name: "tao",
  age: 18,
};
let info = {
  name: "xyj",
  age: 21,
};

finalizationRegistry.register(obj, "aaa");
finalizationRegistry.register(info, "bbb");

obj = null;
info = null;
```

上面的代码在 Node 下是无法看出效果的，Node 帮你运行代码的时候会创建一个进程，当你的代码运行完后，它会自动把你的这个进程关掉

我们可以在浏览器上查看，因为浏览器的话，它有一个专门的进程会一直在这里执行的

GC 是**不定时**的来查看你的代码是否可以清除，如果可以清除就会清除掉


### 二、WeakRefs

如果我们默认将一个对象赋值给另外一个引用，那么这个引用是一个强引用：

我们来看这段代码

```js
const finalizationRegistry = new FinalizationRegistry((value) => {
  console.log(`注册在finalizationRegistry上的${value}对象被GC回收了`);
});

let obj = {
  name: "tao",
  age: 18,
};

let info = obj;

finalizationRegistry.register(obj, "aaa");

obj = null;
```

还是画图吧


如果我们希望是一个弱引用的话，可以使用 WeakRef；

```js
const finalizationRegistry = new FinalizationRegistry((value) => {
  console.log(`注册在finalizationRegistry上的${value}对象被GC回收了`);
});

let obj = {
  name: "tao",
  age: 18,
};

let info = new WeakRef(obj);

finalizationRegistry.register(obj, "aaa");

obj = null;
```


### 三、逻辑运算符和赋值表达式

这个算是逻辑运算符里一些语法糖

```js
// 1.||= 逻辑或赋值运算
let message1 = undefined;
// message = message || 'default value';
message1 ||= "default value";
console.log(message1); // default value

// 2.&&= 逻辑与赋值运算
// 这个其实感觉用的很少
// 一般情况我们都是这样使用 info && info.name
// 用它来赋值的话感觉使用的场景很少
let info = {
  name: "tao",
  age: 18,
};
// 这段代码的意思是先判断你有没有info，如果有就把info里的name赋值给info
// info = info && info.name
info &&= info.name;
console.log(info); // tao

// 3.??= 逻辑空赋值运算
let message2 = NaN;
// message2如果是null/undefined时对其赋值
message2 ??= "default value";
console.log(message2); // NaN
```

### 四、Numeric Separator

数字分隔符可以在数字之间创建可视化分隔符，通过\_下划线来分割数字，使数字更具可读性

```js
const money = 1_000_000_000;
//等价于
const money = 1000000000;
// 1_000_000_000 === 1000000000; // true
```

### 五、String.replaceAll

[replaceAll](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/replaceAll)返回一个全新的字符串，所有符合匹配规则的字符都将被替换掉

```js
const name = "xyj";
console.log(name.replaceAll("r", "")); // codetao
```

### 六、Promise.any

[any](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/any)方法会等到一个 fulfilled 状态，才会决定新 Promise 的状态

```js
const p1 = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject("aaa");
  }, 1000);
});
const p2 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("bbb");
  }, 2000);
});
const p3 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("ccc");
  }, 3000);
});

Promise.any([p3, p2, p1])
  .then((res) => {
    console.log("res", res);
  })
  .catch((err) => {
    console.log("err", err);
  });

// res bbb
```

如果所有的 Promise 都是 reject 的，那么会报一个 AggregateError 的错误。

```js
const p1 = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject("aaa");
  }, 1000);
});
const p2 = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject("bbb");
  }, 2000);
});
const p3 = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject("ccc");
  }, 3000);
});

Promise.any([p3, p2, p1])
  .then((res) => {
    console.log("res", res);
  })
  .catch((err) => {
    console.log("err", err);
    console.log(err.errors);
  });

// err [AggregateError: All promises were rejected]
// [ 'ccc', 'bbb', 'aaa' ]
```

期待 ES13 更新...

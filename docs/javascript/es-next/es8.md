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

## 六、async/await

### 6.1 异步函数 async function

async关键字用于声明一个异步函数：
- async是asynchronous单词的缩写，异步、非同步；
- sync是synchronous单词的缩写，同步、同时；

async异步函数可以有很多中写法：

```js
async function foo1() {

}

const foo2 = async function () {

}

const foo3 = async () => {

}

class Person {
  async foo() {

  }
}
```

### 6.2 异步函数的执行流程

异步函数的内部代码执行过程和普通的函数是一致的，默认情况下也是会被同步执行。

```js
async function foo() {
  console.log('start');
  console.log(1);
  console.log(2);
  console.log(3);
  console.log('end');
}

foo()
console.log('abc');

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

情况一：异步函数也可以有返回值，但是异步函数的返回值会被包裹到Promise.resolve中；

```js
async function foo() {
  console.log('start');
  console.log(1);
  console.log(2);
  console.log(3);
  console.log('end');
}

// 默认没有返回值，那就是return undefined
foo().then(res => {
  console.log(res);
})
console.log('abc');

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

情况二：如果我们的异步函数的返回值是Promise，Promise.resolve的状态会由Promise决定；

```js
async function foo() {
  console.log('start');
  console.log(1);
  console.log(2);
  console.log(3);
  console.log('end');
  return new Promise((resolve, reject) => {
    resolve('aaa')
  })
}

foo().then(res => {
  console.log(res);
})
console.log('abc');

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

情况三：如果我们的异步函数的返回值是一个对象并且实现了thenable，那么会由对象的then方法来决定；

```js
async function foo() {
  console.log('start');
  console.log(1);
  console.log(2);
  console.log(3);
  console.log('end');
  return {
    then(resolve, reject) {
      resolve('bbb')
    }
  }
}

foo().then(res => {
  console.log(res);
})
console.log('abc');

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

如果我们在async中抛出了异常，那么程序它并不会像普通函数一样报错，而是会作为Promise的reject来传递；

```js
async function foo() {
  console.log('start');
  console.log(1);
  console.log(2);
  console.log(3);
  console.log('end');
  throw new Error('抛出异常')
}

foo().catch(err => {
  console.log('捕获到异常');
})
console.log('abc');

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

### 6.3 await关键字

**async函数另外一个特殊之处**就是可以在它内部使用**await关键字**，而**普通函数中是不可以**的。

await关键字有什么特点呢？
- 通常使用await是后面会`跟上一个表达式`，这个表达式会`返回一个Promise`；
- 那么await会`等到Promise的状态变成fulfilled状态`，之后`继续执行异步函数`；

```js
function request() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('aaa')
    }, 1000);
  })
}

async function foo() {
  const res = await request()
  console.log(1, res);
  console.log(2);
  console.log(3);
}

foo()

console.log('abc');

/**
abc
1 aaa
2
3
 */
```

如果await后面是一个普通的值，那么会直接返回这个值；

```js
async function foo() {
  // 其实我觉得内部它这里会把bbb给转为生成一个新的promise的resolve的值然后返回给res
  const res = await 'bbb'
  console.log(1, res);
  console.log(2);
  console.log(3);
}

foo()

console.log('abc');

/**
abc
1 bbb
2
3
 */
```

如果await后面是一个thenable的对象，那么会根据对象的then方法调用来决定后续的值；

```js
async function foo() {
  const res = await {
    then(resolve, reject) {
      resolve('ccc')
    }
  }
  console.log(1, res);
  console.log(2);
  console.log(3);
}

foo()

console.log('abc');

/**
abc
1 ccc
2
3
 */
```

如果await后面的表达式，返回的Promise是reject的状态，那么会将这个reject结果直接作为函数的Promise的
reject值；

```js
function request() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      reject('fff')
    }, 1000);
  })
}


async function foo() {
  const res = await request()
  console.log(1, res);
  console.log(2);
  console.log(3);
}

foo().catch(err => {
  console.log('err', err);
})

console.log('abc');

/**
abc
err fff
 */
```

诶你可能会有些疑惑，后面的1，2，3为什么没有打印

其实你可以理解为上面res后面的三句打印相对于是在then函数里的打印的

之前我们说过async/await其实是Generator的语法糖

都在then里面，那么上面的代码Promise是reject就不会来到then方法里，只会来到catch里面，所以后面的打印是不会执行的


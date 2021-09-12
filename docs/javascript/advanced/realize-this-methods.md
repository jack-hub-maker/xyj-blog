# 实现 apply、call、bind

接下来我们来实现一下 apply、call、bind 函数：

因为这些 API 都是 C++编写的，这里使用 JavaScript 来模拟简单实现一下

!>注意：我们的实现是练习函数、this、调用关系，不会过度考虑一些**边界情况**(egde case),只考虑部分的边界情况，主要是为了模拟

## apply

系统的 apply

```js
function sum(num1, num2) {
  console.log(this);
  return num1 + num2;
}

var result = sum.apply({}, [10, 20]);
console.log(result);
```

自定义的 apply

```js
Function.prototype.gtapply = function (thisArg, argArray) {
  // 1.获取到真正需要调用的函数
  /**
   * 下面我们要通过xx.gtapply的方法来调用
   * 意味着谁调用我们，就会隐式绑定this
   * 不熟的回去看看this指向的文章
   */
  var fn = this;

  // 2.对thisArg转为对象
  /**
   * 对传入的thisArg进行判断，如果传入的是null或者undefined的话改为指向window
   * 这是前面讲的this规则之外的知识
   * 如果不为null或者undefined的话对传入的值进行转为对象
   * 方便后续进行操作
   * 这里只考虑到了部分边界情况，比如我们thisArg传入的对象中有fn类似这些情况就没有进行处理（主要是为了模拟）
   */
  thisArg =
    thisArg !== null && thisArg !== undefined ? Object(thisArg) : window;

  // 3.调用需要被执行的函数
  /**
   * 给当前this添加一个fn的属性为fn函数，这一步是为了后面能够隐式绑定this
   * 接着调用函数然后通过result来接收函数的返回值
   */
  thisArg.fn = fn;
  // 判断arrArray是否有值
  argArray = argArray ?? [];
  // 扩展运算符和剩余参数后续会讲到
  var result = thisArg.fn(...argArray);
  // 调用完函数，删除无意义的fn属性
  delete thisArg.fn;
  // 4.将最终的结果返回出去
  return result;
};

function sum(num1, num2) {
  console.log(this);
  return num1 + num2;
}

var result = sum.gtapply({}, [10, 20]);
console.log(result);
```

## call

系统的 call

```js
function sum(num1, num2) {
  console.log(this);
  return num1 + num2;
}

var result = sum.call({}, 10, 20);
console.log(result);
```

自定义的 call

```js
// call跟apply很相似，我就不重复写逻辑了
Function.prototype.gtcall = function name(thisArg, ...argArray) {
  var fn = this;

  thisArg =
    thisArg !== undefined && thisArg !== null ? Object(thisArg) : window;

  thisArg.fn = fn;
  var result = thisArg.fn(...argArray);
  delete thisArg.fn;
  return result;
};

function sum(num1, num2) {
  console.log(this);
  return num1 + num2;
}

var result = sum.gtcall({}, 10, 20);
console.log(result);
```

## bind

系统的 bind

```js
function sum(num1, num2, num3, num4) {
  console.log(this);
  return num1 + num2 + num3 + num4;
}
var result = sum.bind({}, 10, 20);
var res = result(30, 40);
console.log(res);
```

自定义的 bind

```js
Function.prototype.gtbind = function (thisArg, ...argArray) {
  // 1.获取到真正需要调用的函数
  var fn = this;

  // 2.对thisArg转为对象
  thisArg =
    thisArg !== undefined && thisArg !== null ? Object(thisArg) : window;

  // 3.将函数放在thisArg中进行调用
  /**
   *  系统的bind就会返回一个函数，然后可以调用这个函数
   *  所以我们需要返回一个函数
   */
  return function (...args) {
    thisArg.fn = fn;
    // 特殊：对两个传入的参数进行合并
    var merge = [...argArray, ...args];
    var result = thisArg.fn(...merge);
    delete thisArg.fn;
    // 4.返回结果
    return result;
  };
};

function sum(num1, num2, num3, num4) {
  console.log(this);
  return num1 + num2 + num3 + num4;
}
var result = sum.gtbind({}, 10, 20);
var res = result(30, 40);
console.log(res);
```

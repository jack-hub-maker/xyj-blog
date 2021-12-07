# 防抖和节流

## 一、认识防抖和节流函数

防抖和节流的概念其实最早并不是出现在软件工程中，防抖是出现在**电子元件**中，节流出现在**流体流动**中

- 而 JavaScript 是事件驱动的，大量的操作会触发事件，加入到事件队列中处理。
- 而对于某些频繁的事件处理会造成性能的损耗，我们就可以通过防抖和节流来限制事件频繁的发生；

防抖和节流函数目前已经是前端实际开发中两个非常重要的函数，也是`面试经常被问到的面试题`。

但是很多前端开发者面对这两个功能，有点摸不着头脑：

- 某些开发者根本`无法区分防抖和节流`有什么区别（面试经常会被问到）；
- 某些开发者可以区分，但是`不知道如何应用`；
- 某些开发者会通过一些第三方库来使用，但是`不知道内部原理，更不会编写`；

先来看看什么是防抖？

我们用一副图来理解一下它的过程：

- 当事件触发时，相应的函数并不会立即触发，而是会等待一定的时间；
- 当事件密集触发时，函数的触发会被频繁的推迟；
- 只有等待了一段时间也没有事件触发，才会真正的执行响应函数；

![](https://gitee.com/itsandy/picgo-img/raw/master/JavaScript/防抖函数.jpg)

防抖的应用场景：

- 输入框中频繁的输入内容，搜索或
  者提交信息；
- 频繁的点击按钮，触发某个事件；
- 监听浏览器滚动事件，完成某些特
  定操作；
- 用户缩放浏览器的 resize 事件；

我们都遇到过这样的场景，在某个搜索框中输入自己想要搜索的内容：

比如想要搜索一个 MacBook：

- 当我输入 m 时，为了更好的用户体验，通常会出现对应的联想内容，这些联想内容通常是保存在服务器的，所以需要
  一次网络请求；
- 当继续输入 ma 时，再次发送网络请求；
- 那么 macbook 一共需要发送 7 次网络请求；
- 这大大损耗我们整个系统的性能，无论是前端的事件处理，还是对于服务器的压力;

但是我们需要这么多次的网络请求吗？

- 不需要，正确的做法应该是在合适的情况下再发送网络请求；
- 比如如果用户快速的输入一个 macbook，那么只是发送一次网络请求；
- 比如如果用户是输入一个 m 想了一会儿，这个时候 m 确实应该发送一次网络请求；
- 也就是我们应该监听用户在某个时间，比如 500ms 内，没有再次触发时间时，再发送网络请求；

这就是防抖的操作：只有在某个时间内，没有再次触发某个函数时，才真正的调用这个函数；

接下来我们再看看什么是节流？

我们用一副图来理解一下节流的过程

- 当事件触发时，会执行这个事件的响应函数；
- 如果这个事件会被频繁触发，那么节流函数会按照一定的频率来执行函数；
- 不管在这个中间有多少次触发这个事件，执行函数的频繁总是固定的；

![](https://gitee.com/itsandy/picgo-img/raw/master/JavaScript/节流函数.jpg)

节流的应用场景：

- 监听页面的滚动事件；
- 鼠标移动事件；
- 用户频繁点击按钮操作；
- 游戏中的一些设计；

很多人都玩过类似于飞机大战的游戏

在飞机大战的游戏中，我们按下空格会发射一个子弹：

- 很多飞机大战的游戏中会有这样的设定，即使按下的频率非常快，子弹也会保持一定的频率来发射；
- 比如 1 秒钟只能发射一次，即使用户在这 1 秒钟按下了 10 次，子弹会保持发射一颗的频率来发射；
- 但是事件是触发了 10 次的，响应的函数只触发了一次；

生活中防抖的例子：

- 比如说有一天我上完课，我说大家有什么问题来问我，我会等待五分钟的时间。
- 如果在五分钟的时间内，没有同学问我问题，那么我就下课了；
  - 在此期间，a 同学过来问问题，并且帮他解答，解答完后，我会再次等待五分钟的时间看有没有其他同学问问题；
  - 如果我等待超过了 5 分钟，就点击了下课（才真正执行这个时间）；

生活中节流的例子：

- 比如说有一天我上完课，我说大家有什么问题来问我，但是在一个 5 分钟之内，不管有多少同学来问问题，我只会
  解答一个问题；
- 如果在解答完一个问题后，5 分钟之后还没有同学问问题，那么就下课；

## 二、防抖函数的实现方式

### 1.lodash 库 debounce 方法

lodash 库也为我们提供了实现防抖的函数[debounce](https://lodash.com/docs/4.17.15#debounce)

```html
<input type="text" class="inputEl" />
<script src="https://cdn.jsdelivr.net/npm/lodash@4.17.21/lodash.min.js"></script>
<script>
  const inputEl1 = document.querySelector(".inputEl1");
  const inputEl2 = document.querySelector(".inputEl2");

  let count = 0;
  function inputChange() {
    console.log(`发送了${++count}次网络请求`);
  }

  // 防抖
  inputEl.oninput = _.debounce(inputChange, 2000);
</script>
```

### 2.自定义防抖函数

#### 2.1 实现防抖函数的基本功能

```js
function debounce(fn, delay) {
  // 1.定义一个定时器，保存上一次的定时器
  let timer = null;

  // 2.真正执行的函数
  function _debounce() {
    // 取消前一次的定时器
    if (timer) clearTimeout(timer);
    // 延迟执行
    timer = setTimeout(() => {
      // 外部传入真正执行的函数
      fn();
    }, delay);
  }
  // 3.返回函数
  return _debounce;
}
```

#### 2.2 优化参数和 this 的执行

```js
function debounce(fn, delay) {
  // 1.定义一个定时器，保存上一次的定时器
  let timer = null;

  // 2.真正执行的函数
  function _debounce(...args) {
    // 取消前一次的定时器
    if (timer) clearTimeout(timer);
    // 延迟执行
    timer = setTimeout(() => {
      // 外部传入真正执行的函数
      fn.apply(this, args);
    }, delay);
  }
  // 3.返回函数
  return _debounce;
}
```

#### 2.3 优化取消操作（增加取消功能）

```js
function debounce(fn, delay) {
  // 1.定义一个定时器，保存上一次的定时器
  let timer = null;

  // 2.真正执行的函数
  function _debounce(...args) {
    // 取消前一次的定时器
    if (timer) clearTimeout(timer);
    // 延迟执行
    timer = setTimeout(() => {
      // 外部传入真正执行的函数
      fn.apply(this, args);
    }, delay);
  }

  // 封装取消功能（函数也是一个对象，所以我们可以在函数上添加属性
  _debounce.cancel = () => {
    if (timer) clearTimeout(timer);
    timer = null;
  };
  // 3.返回函数
  return _debounce;
}
```

#### 2.4 优化立即执行效果（第一次立即执行）

```js
function debounce(fn, delay, Immediately = false) {
  // 1.定义一个定时器，保存上一次的定时器
  let timer = null;
  let invoke = false;

  // 2.真正执行的函数
  function _debounce(...args) {
    // 取消前一次的定时器
    if (timer) clearTimeout(timer);

    // 判断是否立即执行一次
    if (Immediately && !invoke) {
      fn.apply(this, args);
      invoke = true;
    }

    // 延迟执行
    timer = setTimeout(() => {
      // 外部传入真正执行的函数
      fn.apply(this, args);
    }, delay);
  }
  _debounce.cancel = () => {
    if (timer) clearTimeout(timer);
    timer = null;
  };
  // 3.返回函数
  return _debounce;
}
```

#### 2.5 优化返回值

```js
function debounce(fn, delay, Immediately = false, resultCallback) {
  // 1.定义一个定时器，保存上一次的定时器
  let timer = null;
  let invoke = false;

  // 2.真正执行的函数
  function _debounce(...args) {
    return new Promise((resolve, reject) => {
      // 取消前一次的定时器
      if (timer) clearTimeout(timer);

      // 判断是否立即执行一次
      if (Immediately && !invoke) {
        const result = fn.apply(this, args);
        if (resultCallback) resultCallback(result);
        resolve(result);
        invoke = true;
      }

      // 延迟执行
      timer = setTimeout(() => {
        // 外部传入真正执行的函数
        const result = fn.apply(this, args);
        if (resultCallback) resultCallback(result);
        resolve(result);
      }, delay);
    });
  }

  _debounce.cancel = () => {
    if (timer) clearTimeout(timer);
    timer = null;
  };

  // 3.返回函数
  return _debounce;
}
```

## 三、节流函数的实现方式

### 1.lodash 库 throttle 方法

lodash 库也为我们提供了实现防抖的函数[throttle](https://lodash.com/docs/4.17.15#throttle)

```html
<input type="text" class="inputEl" />
<script src="https://cdn.jsdelivr.net/npm/lodash@4.17.21/lodash.min.js"></script>
<script>
  const inputEl1 = document.querySelector(".inputEl1");
  const inputEl2 = document.querySelector(".inputEl2");

  let count = 0;
  function inputChange() {
    console.log(`发送了${++count}次网络请求`);
  }

  // 防抖
  inputEl.oninput = _.throttle(inputChange, 2000);
</script>
```

### 2.自定义节流函数

#### 2.1 实现节流函数的基本功能

```js
function throttle(fn, delay) {
  // 1.记录上一次的时间
  let lastTime = 0;

  // 2.事件触发时，真正执行的函数
  function _throttle() {
    // 2.1 获取当前事件触发时的时间
    const nowTime = new Date().getTime();

    // 2.2 使用当前触发的时间和之前的时间间隔以及上一次开始的时间,计算出还需要多长时间需要去触发函数
    const remainTime = delay - (nowTime - lastTime);
    if (remainTime <= 0) {
      // 2.3 真正触发函数
      fn();

      // 2.4 保留上次触发的时间
      lastTime = nowTime;
    }
  }

  return _throttle;
}
```

具体后续的更多功能凭我现在的技术还无法实现，后续更新...



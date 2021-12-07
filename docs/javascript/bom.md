# BOM 浏览器操作

JavaScript 有一个非常重要的运行环境就是浏览器，而且浏览器本身又作为一个应用程序需要对其本身进行操作，所以通常浏览器会有对
应的对象模型（BOM，Browser Object Model）。

我们可以将 BOM 看成是连接 JavaScript 脚本与浏览器窗口的桥梁。

BOM 主要包括一下的对象模型：

- window：包括全局属性、方法，控制浏览器窗口相关的属性、方法；
- location：浏览器连接到的对象的位置（URL）；
- history：操作浏览器的历史；
- document：当前窗口操作文档的对象；

window 对象在浏览器中有两个身份：

1.  全局对象。

- 我们知道 ECMAScript 其实是有一个全局对象的，这个全局对象在 Node 中是 global；
- 在浏览器中就是 window 对象；

2.  浏览器窗口对象。

- 作为浏览器窗口时，提供了对浏览器操作的相关的 API；

## 一、window

### 1.window 全局对象

在浏览器中，window 对象就是之前经常提到的全局对象，也就是我们之前提到过 GO 对象：

- 比如在全局通过 var 声明的变量，会被添加到 GO 中，也就是会被添加到 window 上；
- 比如 window 默认给我们提供了全局的函数和类：setTimeout、Math、Date、Object 等；

```js
var name = "tao";

function foo() {
  console.log("foo");
}

console.log(window.name);
window.foo();

window.setTimeout(() => {
  console.log("setTimeout");
}, 2000);

console.log(new window.Date().getTime());
console.log(new window.Object());
```

### 2.window 窗口对象

事实上[window](https://developer.mozilla.org/zh-CN/docs/Web/API/Window)对象上肩负的重担是非常大的：

- 包含大量的属性，localStorage、console、location、history、screenX、scrollX 等等（大概 60+个属性）；
- 包含大量的方法，alert、close、scrollTo、open 等等（大概 40+个方法）；
- 包含大量的事件，focus、blur、load、hashchange 等等（大概 30+个事件）；
- 包含从 EventTarget 继承过来的方法，addEventListener、removeEventListener、dispatchEvent 方法；

接下来我演示一些常见的属性,方法和事件

```js
// 1.常见的属性

// 浏览器高度
console.log(window.outerHeight);
console.log(window.innerHeight);

// 距离边框的距离
console.log(window.screenX);
console.log(window.screenY);

// 监听方法
window.addEventListener("scroll", () => {
  console.log(window.scrollX, window.scrollY);
});

// 2.常见的方法

const btn = document.querySelector("button");
btn.addEventListener("click", () => {
  // scrollTo
  // window.scrollTo({ top: 300 })
  // open
  // window.open('https://docusaurus-blog-likesandy.vercel.app/', '_self')
  // close
  // window.close()
});

// 3.常见的事件

window.onload = () => {
  console.log("页面加载完毕");
};

window.onfocus = () => {
  console.log("window获得了焦点");
};

window.onblur = () => {
  console.log("window失去了焦点");
};
```

## 二、EventTarget

Window 继承自[EventTarget](https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget)，所以会继承其中的属性和方法：

- addEventListener：注册某个事件类型以及事件处理函数；
- removeEventListener：移除某个事件类型以及事件处理函数；
- dispatchEvent：派发某个事件类型到 EventTarget 上；

```js
const btn1 = document.querySelector(".btn1");
const btn2 = document.querySelector(".btn2");
const btn3 = document.querySelector(".btn3");

function btn1Click() {
  console.log("btn1发生了点击");
}

btn1.addEventListener("click", btn1Click);
btn2.addEventListener("click", () => {
  btn1.removeEventListener("click", btn1Click);
});

btn3.addEventListener("click", () => {
  window.dispatchEvent(new Event("codertao"));
});

window.addEventListener("codertao", () => {
  console.log("监听到了codertao事件");
});
```

更多事件监听：https://developer.mozilla.org/zh-CN/docs/Web/Events

## 三、Location

[Location](https://developer.mozilla.org/zh-CN/docs/Web/API/Location)对象用于表示 window 上当前链接到的 URL 信息。

常见的属性有哪些呢？

- href: 当前 window 对应的超链接 URL, 整个 URL；
- protocol: 当前的协议；
- host: 主机地址；
- hostname: 主机地址(不带端口)；
- port: 端口；
- pathname: 路径；
- search: 查询字符串；
- hash: 哈希值；

```js
console.log(location);
```

我们会发现 location 其实是 URL 的一个抽象实现：

![](https://gitee.com/itsandy/picgo-img/raw/master/JavaScript/location.png)

location 有如下常用的方法：

- assign：赋值一个新的 URL，并且跳转到该 URL 中；
- replace：打开一个新的 URL，并且跳转到该 URL 中（不同的是不会在浏览记录中留下之前的记录）；
- reload：重新加载页面，可以传入一个 Boolean 类型；

```js
const btn = document.querySelector("button");

btn.onclick = () => {
  // location.assign('https://docusaurus-blog-likesandy.vercel.app/')
  // location.replace('https://docusaurus-blog-likesandy.vercel.app/')
  location.reload();
};
```

## 四、history

[history](https://developer.mozilla.org/zh-CN/docs/Web/API/History)对象允许我们访问浏览器曾经的会话历史记录。

有两个属性：

- length：会话中的记录条数；
- state：当前保留的状态值；

有五个方法：

- back()：返回上一页，等价于 history.go(-1)；
- forward()：前进下一页，等价于 history.go(1)；
- go()：加载历史中的某一页；
- pushState()：打开一个指定的地址；
- replaceState()：打开一个新的地址，并且使用 replace；

```js
const btn = document.querySelector("button");
btn.onclick = () => {
  // history.pushState({ name: 'tao' }, '', 'aaa')
  history.replaceState({ name: "tao" }, "", "aaa");
  console.log(history.length);
  console.log(history.state);
};
```

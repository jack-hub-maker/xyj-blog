# JavaScript

## 单线程和异步

## 变量类型和计算

### typeof

typeof 能识别所有值类型,也可以识别函数,但是对于引用类型是不能细分的

```js
typeof NaN === "number";
typeof [1, 2, 4] === "object";
// new都是对象
typeof new Boolean(true) === "object";
// 类的本质是构造函数,构造函数是函数
typeof class C {} === "function";
// JavaScript 诞生以来便如此
typeof null === "object";
```

### ==和===

什么时候用==,什么时候用===

!>绝大多数场合应该使用 === ，只有检测 null/undefined 的时候可以使用 x == null ，因为通常我们不区分 null 和 undefined ，即将 x == null 作为 x === null || x === undefined 的缩写。

```js
// ==的时候前后会进行类型转换
// 会产生一些不符合预期的结果
100 == "100"; // true
0 == ""; // true
0 == false; // true
false == ""; // true
null == undefined; // true
```

### 字符串拼接

- -
- [Array.prototype.join](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/join)
- [String.prototype.concat](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/concat)

性能对比

concat 最快

![](https://gitee.com/itsandy/picgo-img/raw/master/面试/字符串拼接性能对比.png)

### falsely 和 truly

truly 变量:!!a === true 的变量

falsely 变量::!!a === false 的变量

```js
// 以下是falsely变量,除此以外都是truly变量
!!0 === false;
!!undefined === false;
!!NAN === false;
!!"" === false;
!!null === false;
!!false === false;
```

!>JS 中的 if 语句就是通过括号内的是否是 truly 变量来进行判断的

### 判断是否为数组

- instanceof(不准确,如果你从一个框架(比如 iframe)向另一个框架传入一个数组，那么传入的数组与在第二个框架中原生创建的数组分别具有各自不同的构造函数。)
- constructor(不准确,可以重写)
- Array.isArray
- Object.prototype.toString(判断是否=== '[object Array]')

优先 Array.isArray,其次 Object.prototype.toString

### 判断某个字符串以 xx 开头

- indexOf(===0 开头)
- String.prototype.startsWith

## DOM

### 获取元素节点

```js
document.getElementById;
document.getElementsByName;
document.getElementsByTagName;
document.querySelector;
document.querySelectorAll;
```

### property 和 attribute

```js
const box = document.querySelector(".box");
// property
box.style.width = "200px";
box.style.color = "orange";
// attribute
box.setAttribute("name", "tao");
const showBox = box.getAttribute("name");
console.log(showBox); // tao
```

- property:修改对象属性
- attribute:修改 html 属性
- 两者都有可能引起 DOM 重新渲染

### DOM 结构操作

```js
// 新增节点
const divEle = document.createElement("div");
// 移动节点(在body后面插入一个div)
document.body.appendChild(divEle);
// 获取父元素
console.log(divEle.parentNode); // body
// 获取子元素(集合)
console.log(document.body.childNodes); // NodeList
```

### DOM 性能优化

- DOM 查询做缓存(变量保存起来)
- 将频繁的操作改为一次性操作(比如要求创建 10 个 div 然后插入到 body 里)

```js
// 创建一个代码片段
const list = document.createDocumentFragment();

for (let index = 0; index < 10; index++) {
  const divEl = document.createElement("div");
  // 创建出来的节点全部放入代码片段中
  list.appendChild(divEl);
}
// 统一移动节点
document.body.appendChild(list);
```

## BOM

### navigator

```js
// 查看浏览器信息
const ua = navigator.userAgent;
console.log(ua);
const isChrome = ua.includes("Chrome");
console.log(isChrome);
```

### screen

```js
// 获取屏幕信息
console.log(screen.width);
console.log(screen.height);
```

### location

```js
// 查看URL信息
console.log(location.href); // url
console.log(location.protocol); // 协议
console.log(location.pathname); // 路径
console.log(location.port); // 端口
console.log(location.search); // 参数
console.log(location.hash); // 哈希值
```

### history

```js
console.log(history.back()); // 后退
console.log(history.forward); // 前进
```

## 事件

### 事件冒泡

- 基于 DOM 树结构
- 事件会顺着触发元素往上冒泡

[![Go to CodeSandbox](https://camo.githubusercontent.com/90808661433696bc57dce8d4ad732307b5cec6270e6b846f114dcd7ee7f9458a/68747470733a2f2f636f646573616e64626f782e696f2f7374617469632f696d672f706c61792d636f646573616e64626f782e737667)](https://codesandbox.io/s/shy-paper-4w9ne?file=/index.html)

### 事件代理

- 代码简洁
- 减少浏览器内存占用
- 但是,不能滥用

比如无限下拉的图片列表,如何监听每一张图片的点击?就可以使用事件代理

[![Go to CodeSandbox](https://camo.githubusercontent.com/90808661433696bc57dce8d4ad732307b5cec6270e6b846f114dcd7ee7f9458a/68747470733a2f2f636f646573616e64626f782e696f2f7374617469632f696d672f706c61792d636f646573616e64626f782e737667)](https://codesandbox.io/s/boring-tu-jtdyt?file=/index.html)

### 编写一个通用的事件监听函数

## Ajax

### XMLHttpRequest

### 同源策略

ajax 请求时,浏览器要求当前网页和 server 必须同源(安全性)

同源:协议、域名、端口三者必须一致

比如:

- 前端:http://codertao:8080/
- server:https://itsandy:2021/

加载图片 css js 可无视同源策略

- img
- link
- js

### 跨域

- 所有的跨域,都必须经过 server 端的允许和配合
- 未经 server 端允许就会产生跨域

### jsonp

- script 可以绕过跨域的限制
- 服务器可以任意动态拼接数据返回
- 所以,script 就可以拿到跨域的数据,只要 server 端愿意返回

### CORS

- 服务器设置 http header 即可

### 发送网络请求方式

- XMLHttpRequest
- $.ajax(jQuery)
- Fetch(兼容性)
- axios

## 存储

### cookie

- 本身是用于浏览器和 server 通讯
- 被借用到本地存储来
- 可用 document.cookie = ''的方法进行修改

通过 key value 的形式存储用;分割,一次只能**追加**一个,遇到相同的 key 会进行覆盖

![](https://gitee.com/itsandy/picgo-img/raw/master/面试/cookie.png)

cookie 的缺点

- 存储大小,最大 4kb
- http 每次请求时需要发送到服务端,增加请求数据量
- 只能使用 document.cookie = ''的方式进行操作,不方便

### localStorage、sessionStorage

- HTML5 专门为存储而设计的,最大可存 5M
- API 简单易用 setItem、getItem
- 不会随着 http 请求被发送出去

![](https://gitee.com/itsandy/picgo-img/raw/master/面试/localStorage.png)

两者区别

- localStorage 数据会永久存储,除非手动删除
- sessionStorage 数据只存在于当前会话,浏览器关闭则清空

三者区别

- 容量
- API 易用性
- 是否跟随 http 请求发送出去

xss 跨站请求攻击

xsrf(跨站请求伪造)

场景:我在一个网站准备买 id=100 这个商品,然后有人给我发了一封邮件,我点开了这个邮件,我就付款买了 id=200 这个商品

因为我在这个网站是登录的状态,我点开了这个邮件,邮件正文可能是一个图片链接 id=200 的付款按钮地址

预防

- post 接口
- 增加验证(验证码,短信,指纹)

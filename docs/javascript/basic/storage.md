# 浏览器存储方案

## 一、认识 Storage

WebStorage 主要提供了一种机制，可以让浏览器提供一种比 cookie 更直观的 key、value 存储方式：

- [localStorage](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/localStorage)本地存储，提供的是一种永久性的存储方法，在关闭掉网页重新打开时，存储的内容依然保留；
- [sessionStorage](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/sessionStorage)会话存储，提供的是本次会话的存储，在关闭掉会话时，存储的内容会被清除；

```js
localStorage.setItem("name", "tao");
sessionStorage.setItem("name", "sandy");
```

![](https://gitee.com/itsandy/picgo-img/raw/master/JavaScript/localStorage.png)
![](https://gitee.com/itsandy/picgo-img/raw/master/JavaScript/sessionStorage.png)

## 二、localStorage 和 sessionStorage 的区别

我们会发现 localStorage 和 sessionStorage 看起来非常的相似。

那么它们有什么区别呢？

1.  关闭网页后重新打开，localStorage 会保留，而 sessionStorage 会被删除；
2.  在页面内实现跳转，localStorage 会保留，sessionStorage 也会保留；
3.  在页面外实现跳转（打开新的网页），localStorage 会保留，sessionStorage 不会被保留；

因为在浏览器中我们可以理解一个标签页就是一个会话

## 三、Storage 常见的方法和属性

这里我以 localStorage 为例

```js
// 方法
localStorage.setItem("name", "tao");
localStorage.setItem("age", 18);
console.log(localStorage.key(0));

console.log(localStorage.getItem("name")); // tao

localStorage.removeItem("name");
localStorage.clear();

// 属性
console.log(localStorage.length);
```

## 四、封装 Storage

在开发中，为了让我们对 Storage 使用更加方便，我们可以对其进行一些封装：

```js
class TAOCache {
  constructor(isLocal = true) {
    this.storage = isLocal ? localStorage : sessionStorage;
  }
  setItem(key, value) {
    this.storage.setItem(key, JSON.stringify(value));
  }
  getItem(key) {
    const value = this.storage.getItem(key);
    if (value) {
      return JSON.parse(value);
    }
  }
  removeItem(key) {
    this.storage.removeItem(key);
  }
  clear() {
    this.storage.clear();
  }
  key(index) {
    return this.storage.key(index);
  }
  length() {
    return this.storage.length;
  }
}

const localCache = new TAOCache();
const sessionCache = new TAOCache(false);

export { localCache, sessionCache };
```

## 五、认识 IndexedDB

什么是 IndexedDB 呢？

- 我们能看到 DB 这个词，就说明它其实是一种数据库（Database），通常情况下在服务器端比较常见；
- 在实际的开发中，大量的数据都是存储在数据库的，客户端主要是请求这些数据并且展示；
- 有时候我们可能会存储一些简单的数据到本地（浏览器中），比如 token、用户名、密码、用户信息等，比较少存储大量的数
  据；
- 那么如果确实有大量的数据需要存储，这个时候可以选择使用 IndexedDB；

[IndexedDB](https://developer.mozilla.org/zh-CN/docs/Web/API/IndexedDB_API) 是一种底层的 API，用于在客户端存储大量的结构化数据。

- 它是一种事务型数据库系统，是一种基于 JavaScript 面向对象数据库，有点类似于 NoSQL（非关系型数据库）；
- IndexDB 本身就是基于事务的，我们只需要指定数据库模式，打开与数据库的连接，然后检索和更新一系列事务即可；

!>IndexedDB 用的很少很少，后续用到再来做一些补充

## 六、认识 Cookie

?>其实 Cookie 跟服务器的关系是更加精密的，js 一般都不会做任何操作

Cookie（复数形态 Cookies），又称为“小甜饼”。类型为“小型文本文件，某些网站为了辨别用户身份而存储在用户本地终端
（Client Side）上的数据。

浏览器会在特定的情况下携带上 cookie 来发送请求，我们可以通过 cookie 来获取一些信息；

Cookie 总是保存在客户端中，按在客户端中的存储位置，Cookie 可以分为内存 Cookie 和硬盘 Cookie。

- 内存 Cookie 由浏览器维护，保存在内存中，浏览器关闭时 Cookie 就会消失，其存在时间是短暂的；
- 硬盘 Cookie 保存在硬盘中，有一个过期时间，用户手动清理或者过期时间到时，才会被清理；

如果判断一个 cookie 是内存 cookie 还是硬盘 cookie 呢？

- 没有设置过期时间，默认情况下 cookie 是内存 cookie，在关闭浏览器时会自动删除；
- 有设置过期时间，并且过期时间不为 0 或者负数的 cookie，是硬盘 cookie，需要手动或者到期时，才会删除；

一般来说我们登录成功之后，服务器会返回给客户端 Cookie，在 Response Headers 的 Set-Cookie 里存放 cookie，我们下次发送网络请求的时候它会自动把 cookie 携带过去，服务器就会验证 cookie，如果验证通过的话说明用户已经登录了，就可以返回给客户端对应的数据了，如果验证失败了，就说明你没有登录，就会重定向到对应的登录页面

cookie 的缺点

1.  有了 cookie 之后，每次请求都会附加 cookie（流量提高，用户体验差）
2.  明文传输 headers（不太安全，虽然没有绝对的安全，但是能预防就预防）
3.  大小 4kb（对于表单登录的话其实已经足够了）
4.  cookie 验证登录（客户端有很多，浏览器，ios，安卓，小程序，每一个的话服务器可能要设置不同的 cookie 格式）

!>所以现在大多数会使用 token 了，token 里面是有一些算法，会根据当前请求的资源判断是否这次请求需要携带 token 来进行处理的，如果不需要 token 这次请求就无需携带 token

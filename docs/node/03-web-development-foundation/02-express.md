# express

前面我们已经学习了使用 http 内置模块来搭建 Web 服务器，为什么还要使用框架？

- 原生 http 在进行很多处理时，会较为复杂；
- 有 URL 判断、Method 判断、参数处理、逻辑代码处理等，都需要我们自己来处理和封装；
- 并且所有的内容都放在一起，会非常的混乱；

目前在 Node 中比较流行的 Web 服务器框架是 express、koa

我们先来学习 express，后面再学习 koa，并且对他们进行对比；

express 早于 koa 出现，并且在 Node 社区中迅速流行起来：

- 我们可以基于 express 快速、方便的开发自己的 Web 服务器；
- 并且可以通过一些实用工具和中间件来扩展自己功能；

[Express](https://expressjs.com/)整个框架的核心就是**中间件**（暂时可以理解为回调函数），理解了中间件其他一切都非常简单！

## 一、安装

express 的使用过程有两种方式：

- 通过 express 提供的脚手架，直接创建一个应用的骨架；
- 从零搭建自己的 express 应用结构；

安装[express-generator](https://github.com/expressjs/generator)

从零搭建自己的 express 应用结构

```bash
# 初始化项目
npm init -y
# 安装express
npm i express
```

## 二、基本使用

我们来创建第一个 express 项目：

- 我们会发现，之后的开发过程中，可以方便的将请求进行分离：
- 无论是不同的 URL，还是 get、post 等请求方式；
- 这样的方式非常方便我们已经进行维护、扩展；
- 当然，这只是初体验，接下来我们来探索更多的用法；

```js
const express = require("express");

// express其实是一个函数，返回一个app
const app = express();

// 监听默认路径
app.get("/", (req, res, next) => {
  res.end("Hello Express");
});

app.post("/", (req, res, next) => {
  res.end("Hello POST Express");
});

app.get("/login", (req, res, next) => {
  res.end("请先登录");
});

// 开启监听
app.listen(8888, () => {
  // 你可能会好奇为什么这段代码没有执行
  // 因为默认我们访问的是根路径，意味着我们需要监听根路径
  // 根路径中调用了end，返回了数据，整个响应周期就结束了
  console.log("express服务器启动成功");
});
```

## 三、认识中间件

Express 是一个路由和中间件的 Web 框架，它本身的功能非常少：

Express 应用程序本质上是一系列中间件函数的调用；

中间件是什么呢？

- 中间件的本质是传递给 express 的一个回调函数；
- 这个回调函数接受三个参数：
  - 请求对象（request 对象）；
  - 响应对象（response 对象）；
  - next 函数（在 express 中定义的用于执行下一个中间件的函数）；

中间件中可以执行哪些任务呢？

- 执行任何代码；
- 更改请求（request）和响应（response）对象；
- 结束请求-响应周期（返回数据）；
- 调用栈中的下一个中间件；

如果当前中间件功能没有结束请求-响应周期，则必须调用 next()将控制权传递给下一个中间件功能，否则，请求
将被挂起。（可以理解为中间件里必须返回数据或者调用 next 方法）

![](https://gitee.com/itsandy/picgo-img/raw/master/node/express-中间件.png)

## 四、应用中间件

### 4.1 自己编写

那么，如何将一个中间件应用到我们的应用程序中呢？

- express 主要提供了两种方式：app/router.use 和 app/router.methods；
- 可以是 app，也可以是 router，router 我们后续再学习:
- methods 指的是常用的请求方式，比如： app.get 或 app.post 等；

我们先来学习 use 的用法，因为 methods 的方式本质是 use 的特殊情况；

案例一：最普通的中间件

```js
const express = require("express");

// express其实是一个函数，返回一个app
const app = express();

// 理解为把所有的中间件都会压入一个队列里（先进先出）
// 先执行第一个中间件，如果没有结束请求（返回数据）或者是将控制器传递给下一个中间件（调用next），那么就会出错
// end只能执行一次（结束请求返回数据），否则会报错
// 所以一般end我们会在最后一个应用的中间件里进行调用
app.use((req, res, next) => {
  console.log("注册了第一个中间件");
  next();
});

app.use((req, res, next) => {
  console.log("注册了第二个中间件");
  next();
});

app.use((req, res, next) => {
  console.log("注册了第三个中间件");
  res.end("Hello Express");
});

// 开启监听
app.listen(8888, () => {
  // 你可能会好奇为什么这段代码没有执行
  // 因为默认我们访问的是根路径，意味着我们需要监听根路径
  // 根路径中调用了end，返回了数据，整个响应周期就结束了
  console.log("express服务器启动成功");
});
```

案例二：路径中间件

```js
const express = require("express");

const app = express();

app.use((req, res, next) => {
  console.log("注册了第一个中间件");
  next();
});

app.use("/home", (req, res, next) => {
  console.log("home中间件");
});

app.use((req, res, next) => {
  console.log("注册了第二个中间件");
  next();
});

app.listen(8888, () => {
  console.log("express服务器启动成功");
});
```

?>听懂了我前面说的,你应该知道如果我们访问 home 路径,打印的结果会是**注册了第一个中间件和 home 中间件**

案例三: 路径和方法中间件

```js
app.use((req, res, next) => {
  console.log("注册了第一个中间件");
  next();
});

app.post("/login", (req, res, next) => {
  req.on("data", (data) => {
    console.log(data.toString());
  });
  res.end();
});
```

案例四:连续注册中间件

```js
app.get(
  "/home",
  (req, res, next) => {
    console.log("home1");
    next();
  },
  (req, res, next) => {
    console.log("home2");
    next();
  },
  (req, res, next) => {
    console.log("home3");
    next();
  },
  (req, res, next) => {
    console.log("home4");
    res.end();
  }
);
```

讲完中间件的用法,接下来讲讲中间件的应用(json 解析)

**优化前**

```js
// 如果我们需要对json进行解析也许会这样来做
// 如果有很多的接口都是一样的逻辑
// 我们就可以使用中间件来进行封装
app.post("/home", (req, res, next) => {
  req.on("data", (data) => {
    const info = JSON.parse(data.toString());
    console.log(info);
  });
  res.end();
});

app.post("/about", (req, res, next) => {
  req.on("data", (data) => {
    const info = JSON.parse(data.toString());
    console.log(info);
  });
  res.end();
});
```

**优化后**

```js
// 如果我们通过json的方式传递就可以这样来做
// post方法会将数据放到body中
// 我们就可以把json数据放在body里
// 之后调用不同的接口就从body里获取
app.use((req, res, next) => {
  if (req.headers["content-type"] === "application/json") {
    req.on("data", (data) => {
      const info = JSON.parse(data.toString());
      req.body = info;
    });
    req.on("end", () => {
      res.end();
      next();
    });
  } else {
    next();
  }
});

app.post("/home", (req, res, next) => {
  console.log(req.body);
});

app.post("/about", (req, res, next) => {
  console.log(req.body);
});
```

事实上我们可以使用 expres 内置的中间件或者使用`body-parser`来完成

```js
// body-parser在3.x是内置的,在4.x被抽离出去了,在最新的4.16.x又内置成了内置函数
// 它会自动对我们的json数据进行解析
app.use(express.json());

app.post("/home", (req, res, next) => {
  console.log(req.body);
  res.end();
});

app.post("/about", (req, res, next) => {
  console.log(req.body);
  res.end();
});
```

如果我们解析的是 application/x-www-form-urlencoded：

```js
// extended: true表示它使用的是第三方库qs
// extended: false表示它使用的是Node内置模块:query
app.use(express.urlencoded({ extended: true }));
```

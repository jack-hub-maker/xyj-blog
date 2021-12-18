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

[Express](https://expressjs.com/zh-cn/)整个框架的核心就是**中间件**（暂时可以理解为回调函数），理解了中间件其他一切都非常简单！

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

那么，如何将一个中间件应用到我们的应用程序中呢？

- express 主要提供了两种方式：app/router.use 和 app/router.methods；
- 可以是 app，也可以是 router，router 我们后续再学习:
- methods 指的是常用的请求方式，比如： app.get 或 app.post 等；

我们先来学习 use 的用法，因为 methods 的方式本质是 use 的特殊情况；

### 3.1 最普通的中间件

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

### 3.2 路径中间件

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

### 3.3 路径和方法中间件

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

### 3.4 连续注册中间件

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

## 四、应用中间件

讲完中间件的用法,接下来讲讲中间件的应用

### 4.1 json 解析

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

### 4.2 form-data 解析

默认我们是无法获取 form-data 数据的

如果我们想对 form-data 数据来进行解析的话推荐使用 express 提供的第三方库[multer](https://github.com/expressjs/multer)来完成

1.  安装 multer

```sh
pnpm i multer
```

2.  使用 multer

```js
// 导入multer
const multer = require("multer");
// 调用multer返回一个类
const upload = multer();

// 使用upload类中的any方法
app.post("/upload", upload.any(), (req, res, next) => {
  console.log(req.body);
  res.end();
});
```

?>form-data 的话一般现在都应用于**文件上传**,文件上传的话我们就可以使用 upload 里的其它方法

```js
const upload = multer({
  // 在哪里存储文件
  dest: "./upload",
});

// singles上传单个文件,参数:参数的key(参数名)
app.post("/upload", upload.single("file"), (req, res, next) => {
  res.end();
});
```

这样你会发现在 upload 中我们就获取到了上传的文件

![](https://gitee.com/itsandy/picgo-img/raw/master/node/upload.png)

但是还是存在两个问题

1.  文件名不对
2.  文件是无法打开的

这个时候我们就需要对 multer 进行额外的配置

```js
const path = require("path");

// 磁盘存储引擎
const storage = multer.diskStorage({
  // 创建文件夹
  destination: (req, file, cb) => {
    cb(null, "./upload");
  },
  // 确定文件名
  filename: (req, file, cb) => {
    // 这里的文件名我设置的是获取当前的时间戳+文件后缀名
    cb(null, Date.now() + path.extname(file.originalname));
  },
});

const upload = multer({
  storage,
});
```

这样我们就可以正确拿到我们上次的文件了,并且是可以打开的(这里的文件名我这边就简单处理了)

![](https://gitee.com/itsandy/picgo-img/raw/master/node/upload配置成功.png)

?>更多细节请阅读文档,这里只是做个简单的邂逅

### 4.3 保存日志信息

如果我们希望将请求日志记录下来，那么可以使用 express 官网开发的第三方库：[morgan](https://github.com/expressjs/morgan)

1.  安装

```sh
pnpm i morgan
```

2.  使用

```js
const fs = require("fs");

const morgan = require("morgan");

// 这里一定要存在access.log这个文件哦,它是不会帮你创建的
const logList = fs.createWriteStream("./logs/access.log", {
  flags: "a+",
});

app.use(morgan("combined", { stream: logList }));
```

![](https://gitee.com/itsandy/picgo-img/raw/master/node/morgan日志.png)

## 五、request 参数解析

客户端传递到服务器参数的方法常见的是 5 种：

- 通过 get 请求中的 URL 的 params；
- 通过 get 请求中的 URL 的 query；
- 通过 post 请求中的 body 的 json 格式（中间件中已经使用过）；
- 通过 post 请求中的 body 的 x-www-form-urlencoded 格式（中间件使用过）；
- 通过 post 请求中的 form-data 格式（中间件中使用过）；

**params**

```js
// 请求地址: http://127.0.0.1:8888/home/codertao/18
app.get("/home/:name/:age", (req, res, next) => {
  console.log(req.params);
  res.end();
});
```

**query**

```js
// 请求地址: http://127.0.0.1:8888/home?name=tao&age=18
app.get("/home", (req, res, next) => {
  console.log(req.query);
  res.end();
});
```

## 六、response 响应结果

这部分没什么好说的，直接来看案例

```js
app.get("/home", (req, res, next) => {
  // 如果我们要返回一个json数据
  // res.contentType = 'application/json'
  // res.end(JSON.stringify({ name: 'tao', age: 18 }))

  // 设置返回状态码
  res.status(202);

  // express其实为我们提供了更简洁的方法
  res.json({ name: "tao", age: 18 });
});
```

更多响应方式,请点击[这里](https://expressjs.com/zh-cn/4x/api.html#res)查看官方文档

## 七、路由的使用

如果我们将所有的代码逻辑都写在 app 中，那么 app 会变得越来越复杂：

- 一方面完整的 Web 服务器包含非常多的处理逻辑；
- 另一方面有些处理逻辑其实是一个整体，我们应该将它们放在一起：比如对 users 相关的处理
  - 获取用户列表；
  - 获取某一个用户信息；
  - 创建一个新的用户；
  - 删除一个用户；
  - 更新一个用户；

我们可以使用 [express.Router](https://expressjs.com/zh-cn/guide/routing.html) 来创建一个路由处理程序：

- 一个 Router 实例拥有完整的中间件和路由系统；
- 因此，它也被称为 **迷你应用程序**（mini-app）；

```js
// routes/users.js
const express = require("express");

const userRouter = express.Router();

// 这里的路径对应的就是/user
userRouter.get("/", (req, res, next) => {
  res.json("用户信息");
});

// 这里的路径对应的就是/user/create
userRouter.post("/create", (req, res, next) => {
  res.json("创建用户");
});

module.exports = userRouter;
```

```js
// app.js
// 导入路由文件
const userRouter = require("./routes/users");

// 使用中间件
app.use("/user", userRouter);
```

## 八、静态资源服务器

Node 也可以作为静态资源服务器，并且 express 给我们提供了方便部署静态资源的方法；

```js
// 放入需要部署对应的路径
app.use(express.static("./build"));
```

## 九、错误处理

比如我们用户登录，如果密码输错了，那我们该如何来对错误进行处理喃

```js
const THIS_ACCOUNT_DOES_NOT_EXIST = "THIS_ACCOUNT_DOES_NOT_EXIST";

app.post("/login", (req, res, next) => {
  // 模拟不能登录
  const isLogin = true;
  if (!isLogin) res.json("登录成功");
  // next传递了参数就会走err中间件
  else next(new Error(THIS_ACCOUNT_DOES_NOT_EXIST));
});

// 传递4个参数就等于err中间件
app.use((err, req, res, next) => {
  let status = 400;
  let message;
  switch (err.message) {
    case THIS_ACCOUNT_DOES_NOT_EXIST:
      message = "this account does not exist";
      break;
    default:
      message = "password input error";
  }
  res.json({
    errCode: message,
    errStatus: status,
  });
});
```

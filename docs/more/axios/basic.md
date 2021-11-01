# Axios

## 一、前言

网络请求是前端中非常重要的内容,学过 `ajax` 的都知道它的重要性

目前前端中发送网络请求的方式有很多种：

这么多的方式我们究竟该选哪一个好喃

## 二、网络请求的方式

### 2.1 Ajax

传统的 Ajax 是基于 XMLHttpRequest(XHR)

为什么不用它呢?

1.  非常好解释, 配置和调用方式等非常混乱
2.  编码起来看起来就非常蛋疼
3.  所以真实开发中很少直接使用, 而是使用 jQuery-Ajax

### 2.2 jQuery-Ajax

在以前的前端开发中,我们我们经常会使用 jQuery-Ajax

jQuery-Ajax 相对于传统的 Ajax 非常好用

为什么不选择它呢？

1.  jQuery 整个项目太大，单纯使用 ajax 却要引入整个 JQuery 非常的不合理（采取个性化打包的方案又不能享受 CDN 服务）
2.  基于原生的 XHR 开发，XHR 本身的架构不清晰，已经有了 fetch 的替代方案
3.  尽管 JQuery 对我们前端的开发工作曾有着深远的影响，但是的确正在推出历史舞台

### 2.3 Fetch

选择或者不选择它?

1.  `Fetch` 是 AJAX 的替换方案，基于 Promise 设计，很好的进行了关注分离，有很大一批人喜欢使用 fetch 进行项目开发
2.  但是 Fetch 的缺点也很明显，首先需要明确的是 Fetch 是一个 low-level（底层）的 API，没有帮助你封装好各种各样的功能
    和实现
3.  比如发送网络请求需要自己来配置 Header 的 Content-Type，不会默认携带 cookie 等
4.  比如错误处理相对麻烦（只有网络错误才会 reject，HTTP 状态码 404 或者 500 不会被标记为 reject）
5.  比如不支持取消一个请求，不能查看一个请求的进度等等

MDN Fetch 学习地址:[https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API/Using_Fetch](https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API/Using_Fetch)

### 2.4 Axios

1.  axios 是目前前端使用非常广泛的网络请求库，包括 Vue 作者也是推荐在 vue 中使用 axios(很早以前)
2.  主要特点包括：在浏览器中发送 XMLHttpRequests 请求、在 node.js 中发送 http 请求、支持 Promise API、拦截请求和
    响应、转换请求和响应数据等等
3.  `axios`: ajax i/o system



## 三、axios 的基本使用

首先介绍两个网站

1.  [Axios 官网](https://axios-http.com/zh/)
2.  [测试网站](http://httpbin.org/)

接下来我们就来简单的来演示使用 axios 发送网络请求

这里我用的是一个接口来进行演示

```js
axios({
  url: "http://httpbin.org/get",
}).then((res) => {
  console.log(res);
});
```

通过 axios 的实例调用,可以传入一个 url,实例本身返回一个 **Promise**,成功请求到数据之后会在内部把状态修改为 reslove,之后就可以调用 then 方法,拿到请求过来的数据

如果你想指定请求的类型的话,可以在 method 写上对应的请求方式 GET/POST(**默认**是 GET 请求)

```js
axios({
  url: "http://httpbin.org/post",
  method: "POST",
}).then((res) => {
  console.log(res);
});
```

我们不仅可以使用上面这种形式发送网络请求,axios 还为我们提供了很多方法,比如 axios.get,axios.post 等

```js
axios.get("http://httpbin.org/get", {}).then((res) => {
  console.log(res);
});
axios.post("http://httpbin.org/post", {}).then((res) => {
  console.log(res);
});
```

如果 url 的后面想跟参数的话也是可以的,而且我们不会直接在后面添加参数,而是把参数写在 params 里

```js
axios({
  url: "http://httpbin.org/get",
  params: {
    name: "gt",
    age: 20,
  },
}).then((res) => {
  console.log(res);
});
```

## 四、axios 的并发请求

如果你想同时发送多个请求的话,可以使用 axios.all

```js
axios
  .all([
    axios.get("http://httpbin.org/get"),
    axios.post("http://httpbin.org/post"),
  ])
  .then(
    axios.spread((res1, res2) => {
      console.log(res1);
      console.log(res2);
    })
  );
```

这个方法会等待多个请求**都完成**之后,才会执行对应的 then 或者 catch 方法

axios.all 返回的结果是一个数组,使用 axios.spread 可以将数组展开来(类似于数组的展开运算符)

## 五、axios 的默认配置

在前面的的实例中,我们的 BaseURL 是固定的

1.  事实上在开发中可能很多参数都是固定的
2.  这个时候我们可以进行一些抽取,也可以利用 axios 的默认配置

```js
axios.defaults.baseURL = "http://httpbin.org";
axios.defaults.timeout = 2000;
axios.get("/get").then((res) => {
  console.log(res);
});
axios.post("/post").then((res) => {
  console.log(res);
});
```

这里列举了两个比较常用的全局配置,具体还有很多配置,如果开发有需求的话可以去查一下文档

## 六、axios 的实例

当我们的项目越来越大,业务也越来越复杂的时候,往往单独一个全局实例是无法满足我们的需求的,这个时候就可以单独创建一个新的实例

创建对应的 axios 的实例

```js
const instance = axios.create({
  baseURL: "http://httpbin.org",
  timeout: 3000,
});
instance({
  url: "/ip",
}).then((res) => {
  console.log(res);
});
```

## 七、axios 的拦截器

axios 提供了拦截器,用于我们在发送每次请求或者得到相应的东西后,进行对应的处理

axios 拦截器配置了**请求**和**响应**两种拦截

那么拦截的意义是什么喃

1.  比如我们请求的数据中的 config 的一些信息不符合服务器的要求,我们就可以拦截下来然后修改,最后返回出去
2.  比如当我们请求的数据很多的时候,界面上就会等待,出现白屏的效果,用户体验就很差,我们可以把数据拦截下来然后界面显示一个 Loding 的组件,当数据请求完毕之后,再把数据返回出去,然后把这个 Loding 组件取消掉
3.  比如我们在某些网络请求的时候,如果用户没有登录,没有携带对应的 token,那么就拦截这个请求,然后提示用户登录或者跳转到登录界面

有一点**很重要**,我们拦截了之后一定要把数据给 return 出去,不然外面无法接收到数据

```js
// 请求拦截
instance.interceptors.request.use(
  (config) => {
    return config;
  },
  (err) => {
    console.log(err);
  }
);
// 响应拦截
instance.interceptors.response.use(
  (res) => {
    return res;
  },
  (err) => {
    console.log(err);
  }
);
```

!> axios 的整个流程：发送网络请求->拦截请求->响应拦截->处理数据

## 八、axios的封装

### 8.1为什么要封装？

为什么我们要对 axios 进行二次封装喃?

1.  默认情况下我们是可以直接使用 axios 来进行开发的
2.  但是我们需要考虑一个问题,假如你的项目中有很多的代码都直击依赖 axios,突然有一天 axios 出现了严重的 bug,不再进行维护了,这个时候你岂不是的一行一行去改(🤣 那岂不是加班加到爽)
3.  大多数的情况下我们会寻求一个新的网络请求的库或者自己进行二次封装
4.  但是有很多的代码都直接依赖了 axios,这方便我们进行修改吗,所以我们要对 axios 进行封装
5.  而且**作为程序员**也要养成封装的思想,这对与我们开发和维护有很大的帮助

接下来我们就对 axiox 进行一个简单的封装

我的习惯的话是在 src 的目录下创建一个 service 的目录,然后在这里面封装对应的网络请求相关的代码

我们先来看一个叫做环境变量的东西,有些情况下我们需要根据不同的环境设置不同的环境变量

这方便我们编写 axios 的配置

### 8.2 环境配置

我们需要根据不同的环境设置不同的环境变量

1.  开发环境:development
2.  生产环境:production
3.  测试环境:test

如何区分不同的环境变量喃?常见的话有三种方法

1.  [手动修改不同的变量](#_821-手动修改)
2.  [根据 process.env.NODE_ENV 的值来进行区分](#_822-根据环境变量值做判断)
3.  [编写不同的环境变量配置文件](#_823-编写相应的配置文件)

#### 8.2.1 手动修改

比如我们在 config 文件编写三个 BASE_URL(开发阶段,生产阶段和测试阶段)

在开发阶段的时候就把生产阶段和测试阶段的 BASE_URL 注释掉,当到了生产阶段的时候就把其它两个 BASE_URL 注释掉

```js
// 开发阶段
export const BASE_URL = "http://gt.com";
// 生产阶段
// export const BASE_URL = 'http://tao.com'
// 测试阶段
// export const BASE_URL = 'http://sandy.com'
```

可见手动修改的方式是很蠢的,所以不推荐使用这种方法

#### 8.2.2 根据环境变量值做判断

根据 process.env.NODE_ENV 的值来进行判断,这个 process.env.NODE_ENV 在不同的环境下面是有不同的值的,如果你使用的是 webpack 来进行开发的话,webpack 的 DefinePlugin 插件会给这个 process.env.NODE_ENV 在不同的环境下注入不同的值

那么我们就可以对这个值做一个判断,让它在不同的环境下设置不同的 BaseURL

```js
let BASE_URL = "";
if (process.env.NODE_ENV === "development") {
  BASE_URL = "http://gt.com";
} else if (process.env.NODE_ENV === "production") {
  BASE_URL = "http://tao.com";
} else {
  BASE_URL = "http://sandy.com";
}
export { BASE_URL };
```

在开发中这种方法是**比较推荐**的

#### 8.2.3 编写相应的配置文件

如果你是用 VueCLI 开发项目的话,可以在根目录创建三个文件配置不同的 BASE_URL

这三个文件会在不同的阶段自动进行读取的

1.  .env.development(开发阶段)
2.  .env.production(生产阶段)
3.  .env.test(测试阶段)

详情可见:[https://cli.vuejs.org/zh/guide/mode-and-env.html#%E6%A8%A1%E5%BC%8F](https://cli.vuejs.org/zh/guide/mode-and-env.html#%E6%A8%A1%E5%BC%8F)


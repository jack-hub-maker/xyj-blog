## 前言

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

## 环境配置

我们需要根据不同的环境设置不同的环境变量

1.  开发环境:development
2.  生产环境:production
3.  测试环境:test

如何区分不同的环境变量喃?常见的话有三种方法

1.  [手动修改不同的变量](#手动修改)
2.  [根据 process.env.NODE_ENV 的值来进行区分](#根据环境变量值做判断)
3.  [编写不同的环境变量配置文件](#编写相应的配置文件)

### 手动修改

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

### 根据环境变量值做判断

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

### 编写相应的配置文件

如果你是用 VueCLI 开发项目的话,可以在根目录创建三个文件配置不同的 BASE_URL

这三个文件会在不同的阶段自动进行读取的

1.  .env.development(开发阶段)
2.  .env.production(生产阶段)
3.  .env.test(测试阶段)

详情可见:[https://cli.vuejs.org/zh/guide/mode-and-env.html#%E6%A8%A1%E5%BC%8F](https://cli.vuejs.org/zh/guide/mode-and-env.html#%E6%A8%A1%E5%BC%8F)

这种方法开发 vue 的话也是可以的

## 代码封装

### axios+js

占位

### axios+ts

占位

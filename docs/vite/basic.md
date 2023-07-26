# Vite

## 一、认识 vite

- Webpack 是目前整个前端使用最多的构建工具，但是除了 webpack 之后也有其他的一些构建工具：
  - 比如 rollup、parcel、gulp、vite 等等
- 什么是 vite 呢？ 官方的定位：**下一代**前端开发与构建工具；
- 如何定义下一代开发和构建工具呢？
  - 我们知道在实际开发中，我们编写的代码往往是不能被浏览器直接识别的，比如 ES6、TypeScript、Vue 文件等
    等；
  - 所以我们必须通过构建工具来对代码进行转换、编译，类似的工具有 webpack、rollup、parcel；
  - 但是随着项目越来越大，需要处理的 JavaScript 呈指数级增长，模块越来越多；
  - 构建工具需要很长的时间才能开启服务器，HMR(热更新) 也需要几秒钟才能在浏览器反应出来；
  - 所以也有这样的说法：天下苦 webpack 久矣；
- Vite (法语意为 "**快速的**"，发音 /vit/) 是一种新型前端构建工具，能够显著提升前端开发体验。

## 二、vite 的构造

- 它主要由两部分组成：
  - 一个开发服务器，它基于原生 ES 模块提供了丰富的内建功能，HMR 的速度非常快速；
  - 一套构建指令，它**使用 rollup**打包我们的代码，并且它是预配置的，可以输出生成环境的优化过的静态资源；
- 目前是否要大力学习 vite？vite 的未来是怎么样的？
  - 我个人非常看好 vite 的未来，也希望它可以有更好的发展；
  - 但是，目前 vite 虽然已经更新到 2.x，依然并不算非常的稳定，并且比较少大型项目（或框架）使用 vite 来进行
    构建；
  - vite 的整个社区插件等支持也还不够完善；
  - 包括 vue 脚手架本身，目前也还没有打算迁移到 vite，而依然使用 webpack（虽然后期一定是有这个打算的）；
  - 所以 vite 看起来非常的火热，在面试也可能会问到，但是实际项目中应用的还比较少；

详情：https://cn.vitejs.dev/

接下来就让我带你走入 Vite 的世界,使用 Vite 在构建项目的时候你会感觉前所未有的**爽**

!> webpack 的生态还是很好,推荐 Vite 目前用于**个人开发**,**团队开发**还是使用 webpack

## 三、vite 脚手架工具

本节并不是 0 基础学习 Vite，使用过 Vue CLI 算起起步门槛，所以这里就直接使用 vite 的脚手架工具来进行演示

在开发中，我们不可能所有的项目都使用 vite 从零去搭建，比如一个 react 项目、Vue 项目；

- 这个时候 vite 还给我们提供了对应的脚手架工具；
- 所以 Vite 实际上是有两个工具的：
  - vite：相当于是一个构建工具，类似于 webpack、rollup；
  - @vitejs/create-app：类似 vue-cli、create-react-app；
- 如果使用脚手架工具呢？

```sh
npm init @vitejs/app <project-name>
```

上面的做法相当于省略了安装脚手架的过程

等价于

```sh
npm install @vitejs/create-app -g create-app
```

![12.png](https://img11.360buyimg.com/ddimg/jfs/t1/178292/33/18532/45265/61122c4eE44152a5c/7a4ab4b21616c9ff.png)
![13.png](https://img12.360buyimg.com/ddimg/jfs/t1/193245/39/17759/87443/61122c4eEaaab326a/46b724d8830e59c2.png)
![14.png](https://img11.360buyimg.com/ddimg/jfs/t1/182334/32/18487/48025/61122c4fE7a25fa3c/03648a1429ce213c.png)

## 四、CSS 的支持

vite 天然支持 css，可以直接编写 css，无需配置

详情：https://cn.vitejs.dev/guide/features.html#css

## 五、TypeScript 的支持

vite 天然支持 TypeScript，可以直接编写 TypeScript，无需配置

你会觉得很神奇，居然默认支持，这是为什么？

如果我们查看浏览器中的请求，会发现请求的依然是 ts 的代码:

- 这是因为 vite 中的服务器 Connect 会对我们的请求进行转发;
- 获取 ts 编译后的代码，给浏览器返回，浏览器可以直接进行解析；

!>注意：在 vite2 中，已经不再使用 Koa 了，而是使用 Connect 来搭建的服务器

由于大多数逻辑应该通过钩子而不是中间件来完成,因此对中间件的需求大大减少,内部服务器应该用现在是一个很好的旧版的 connect 实例,而不是 Koa

**个人观点-Vite 的核心原理**

Vite 会在本地建立一个 Connect 服务器,Vite 中的 Connect 服务器会对我们的请求进行转发比如这里我请求的是 foo.ts 文件,Connect 会将它转发成 foo.ts,虽然两个的名字还是一样的,但是里面的代码被转发成了 es6 的 js 代码,不是因为 ts 才会转为 js 代码,比如你请求的 less 文件,最后也会转发成 es6 的 js 代码,然后将 js 代码返回给浏览器让浏览器解析.第一次我们安装了新的依赖运行的时候,Vite 会帮我们把这个包给存储起来(预打包),下次我们再运行的时候,就会把上次打包过的包直接拿来用,就不需要重新打包

## 六、Vue 的支持

vite 对 vue 提供第一优先级支持：

- Vue 3 单文件组件支持：[@vitejs/plugin-vue](https://github.com/vitejs/vite/tree/main/packages/plugin-vue)
- Vue 3 JSX 支持：[@vitejs/plugin-vue-jsx](https://github.com/vitejs/vite/tree/main/packages/plugin-vue-jsx)
- Vue 2 支持：[underfin/vite-plugin-vue2](https://github.com/underfin/vite-plugin-vue2)

## 七、ESBuild

使用了 vite，你会发现 vite 构建项目是真的快，这是因为 vite 使用 ESBuild 进行预构建

ESBuild 的构建速度和其他构建工具速度对比：

![11.png](https://img10.360buyimg.com/ddimg/jfs/t1/203347/36/673/32594/61122c4eE62947cdf/b5e8b8e7244c4dde.png)

详情：https://esbuild.github.io/

ESBuild 为什么这么快呢？

- 使用 Go 语言编写的，可以直接转换成机器代码，而无需经过字节码；
- ESBuild 可以充分利用 CPU 的多内核，尽可能让它们饱和运行；
- ESBuild 的所有内容都是从零开始编写的，而不是使用第三方，所以从一开始就可以考虑各种性能问题；

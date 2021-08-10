## 关于

本章根据 **coderwhy 老师**腾讯课堂的视频讲解进行的笔记记录,感兴趣的可以去[腾讯课堂](https://ke.qq.com/course/3453141):tada::tada:查看 coderwhy 老师的课程

本章的代码已上传 [GitHub](https://github.com/likesandy/learn-webpack),感兴趣的可以下载然后配合笔记更佳

本章先对 webpack 进行一次简单的邂逅,后期还会深入 webpack 的学习,敬请期待:rose::rose:

遇到很多问题以及更多细节相关的方面请记住多看:books:[官方文档](https://webpack.docschina.org/)

## 认识 webpack

- 事实上随着前端的快速发展，目前前端的开发已经变的越来越复杂了：

  - 比如开发过程中我们需要通过**模块化的方式**来开发；
  - 比如也会使用一些**高级的特性来加快我们的开发效率或者安全性**，比如通过 ES6+、TypeScript 开发脚本逻辑，
    通过 sass、less 等方式来编写 css 样式代码；
  - 比如开发过程中，我们还希望**实时的监听文件的变化**来并且**反映到浏览器上**，提高开发的效率；
  - 比如开发完成后我们还需要**将代码进行压缩**、**合并以及其他相关的优化**；

- 但是对于很多的前端开发者来说，并不需要思考这些问题，日常的开发中根本就没有面临这些问题：
  - 这是因为目前前端开发我们通常都会直接使用三大框架来开发：**Vue、React、Angular**；
  - 但是事实上，这三大框架的创建过程我们都是**借助于脚手架（CLI）的**；
  - 事实上 Vue-CLI、create-react-app、Angular-CLI 都是**基于 webpack** 来帮助我们支持模块化、less、
    TypeScript、打包优化等的；

## Webpack 到底是什么呢？

- 我们先来看一下官方的解释：
  - webpack is a **static module bundler** for **modern** JavaScript applications.
- webpack 是一个静态的模块化打包工具，为现代的 JavaScript 应用程序；
- 我们来对上面的解释进行拆解：
  - **打包 bundler**：webpack 可以将帮助我们进行打包，所以它是一个打包工具
  - **静态的 static**：这样表述的原因是我们最终可以将代码打包成最终的静态资源（部署到静态服务器）；
  - **模块化 module**：webpack 默认支持各种模块化开发，ES Module、CommonJS、AMD 等；
  - **现代的 modern**：我们前端说过，正是因为现代前端开发面临各种各样的问题，才催生了 webpack 的出现和发
    展；

## Webpack 的使用前提

- webpack 的官方文档是[https://webpack.js.org/](https://webpack.js.org/)
  - webpack 的中文官方文档是[https://webpack.docschina.org/](https://webpack.docschina.org/)
- Webpack 的运行是依赖 **Node 环境**的，所以我们电脑上必须有 Node 环境
  - Node 官方网站：[https://nodejs.org/](https://nodejs.org/)

## Webpack 的安装

- webpack 的安装目前分为两个：**webpack**、**webpack-cli**
- 那么它们是什么关系呢？
  - 执行 webpack 命令，会执行 node_modules 下的.bin 目录下的 webpack；
  - webpack 在执行时是依赖 webpack-cli 的，如果没有安装就会报错；
  - 而 webpack-cli 中代码执行时，才是真正利用 webpack 进行编译和打包的过程；
  - 所以在安装 webpack 时，我们需要同时安装 webpack-cli（第三方的脚手架事实上是没有使用 webpack-cli 的，而是类似于自
    己的 vue-service-cli 的东西）

```sh
npm install webpack webpack-cli –g # 全局安装
npm install webpack webpack-cli –D # 局部安装
```

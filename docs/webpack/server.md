
## 为什么要搭建本地服务器？

- 目前我们开发的代码，为了运行需要有两个操作：
  - 操作一：**npm run build**，编译相关的代码；
  - 操作二：通过**live server**或者直接通过浏览器，打开 index.html 代码，查看效果；
- 这个过程经常操作会影响我们的开发效率，我们希望可以做到，当文件发生变化时，可以自动的完成 编译 和 展示；
- 为了完成自动编译，webpack 提供了几种可选的方式：
  - webpack watch mode；
  - webpack-dev-server（常用）；
  - webpack-dev-middleware；

## Webpack watch

- webpack 给我们提供了 watch 模式：
  - 在该模式下，webpack 依赖图中的所有文件，只要有一个**发生了更新**，那么代码将**被重新编译**；
  - 我们**不需要手动**去运行 npm run build 指令了；
- 如何开启 watch 呢？两种方式：
  - 方式一：在导出的配置中，添加 watch: true；
  - 方式二：在启动 webpack 的命令中，添加 --watch 的标识；

```js
wathc: true,
```

```json
"scripts": {
  "watch": "webpack --watch"
},
```

## webpack-dev-server

- 上面的方式可以监听到文件的变化，但是事实上它本身是没有自动刷新浏览器的功能的：
  - 当然，目前我们可以在 VSCode 中使用 live-server 来完成这样的功能；
  - 但是，我们希望在**不使用 live-server**的情况下，可以具备**live reloading（实时重新加载**）的功能；
- 安装 webpack-dev-server

```sh
npm install webpack-dev-server -D
```

- 添加 script 脚本配置

```json
"scripts": {
  "dev": "webpack serve"
},
```

![](/pack/webpack/33.png)

这里的意思是如果我们使用了 webpack-dev-server,因为它这里面会自动箭头 watch 的变化,所以在配置里面就不需要配置 watch 了

```js
// watch: true,
```

- 修改配置文件，告知 dev server，从什么位置查找文件：

我们以前不是配置了 CopyWebpackPlugin 的插件吗,如果我们不想配置这个插件,而且想使用 public 文件夹中的文件的话

那么就可以对 devServer 进行配置

```js
// new CopyWebpackPlugin({
//   patterns: [
//     {
//       from: "public",
//       globOptions: {
//         ignore: [
//           "**/index.html"
//         ]
//       }
//     }
//   ]
// }),
```

```js
devServer: {
  // 如果有些资源不是从webpac中得到服务的话就会从这里进行加载
  contentBase: '/public'
},
```

![](/pack/webpack/34.png)

我画了一个很草率的图来说明 CopyWebpackPlugin 和 content 的差别

![](/pack/webpack/35.png)

那是不是就表明这个 CopyWebpackPlugin 的插件就没有用了喃?

- 当然不是的,一般情况下在开发阶段我们使用 contentBase,生产阶段我们使用 CopyWebpackPlugin 插件
- 因为开发阶段如果我们想拷贝的资源很大(比如 10Gb 的 mp4 文件),这个时候如果我们用 CopyWebpackPlugin 插件,表示我们每次打包之后的资源就很变得很大,我们在加载代码的时候有些时候就会加载不到资源,这个时候就可以使用 contentBase
- 在上线阶段,因为我们最终部署的时候是把代码部署到一个服务器上的,所以要把所有的资源打包到一个地方,所以就用 CopyWebpackPlugin

- webpack-dev-server 在编译之后**不会写入到任何输出文件**，而是将 bundle 文件**保留在内存**中：
  - 当我们开启本地服务的时候,会对当前项目进行一个打包,打包之后将打包之后的资源先放在内存中,内部是 express 服务器
  - 当我们用 express 来访问之前打包到内存的静态资源,如果浏览器去访问的时候也是从内存中读取资源再返回到浏览器
  - 那为什么不直接输出到文件夹喃?岂不是更干脆
  - 如果用户进行访问,会通过 express 服务器把资源读取到内存里面,再转化为对应的数据流再返回给浏览器
  - webpack 直接打包到内存中,放在内存中就少了从文件读取到内存的过程,这样提供服务的服务器性能就会更高一点,用户访问的速度就会更快一点
  - 事实上 webpack-dev-server 使用了一个库叫 memfs（以前用的自己写的 memory-fs 库 ）

## 认识模块热替换（HMR）

- 什么是 HMR 呢？
  - HMR 的全称是**Hot Module Replacement**，翻译为**模块热替换**；
  - 模块热替换是指在 **应用程序运行过程中，替换、添加、删除模块**，而**无需重新刷新整个页面**；
- HMR 通过如下几种方式，来提高开发的速度：
  - **不重新加载整个页面，这样可以保留某些应用程序的状态不丢失**；
  - 只更新**需要变化的内容，节省开发的时间**；
  - 修改了**css、js 源代码**，会**立即在浏览器更新**，相当于直接在浏览器的 devtools 中直接修改样式；
- 如何使用 HMR 呢？
  - 默认情况下，**webpack-dev-server 已经支持 HMR**，我们只需要开启即可；
  - 在不开启 HMR 的情况下，当我们修改了源代码之后，整个页面会自动刷新，使用的是 live reloading；

## 开启 HMR

- 修改 webpack 的配置：

```js
devServer: {
  hot: true
},
```

- 浏览器可以看到如下效果：

![](/pack/webpack/36.png)

- 但是你会发现，当我们修改了某一个模块的代码时，依然是刷新的整个页面：
  - 这是因为我们需要去指定哪些模块发生更新时，进行 HMR；

```js
if (module.hot) {
  module.hot.accept("./js/element.js", () => {
    console.log("element文件更新了");
  });
}
```

## 框架的 HMR

- 有一个问题：在开发其他项目时，我们是否需要经常手动去写入 module.hot.accpet 相关的 API 呢？
  - 比如**开发 Vue、React 项目**，我们**修改了组件**，希望**进行热更新**，这个时候**应该如何去操作**呢？
  - 事实上社区已经针对这些有很成熟的解决方案了；
  - 比如 vue 开发中，我们使用**vue-loader**，此 loader 支持 vue 组件的 HMR，提供开箱即用的体验；
  - 比如 react 开发中，有 React Hot Loader，实时调整 react 组件（目前 React 官方已经弃用了，改成使用**reactrefresh**）；

## HMR 的原理

- 那么 HMR 的原理是什么呢？如何可以做到只更新一个模块中的内容呢？
  - webpack-dev-server 会创建两个服务：**提供静态资源的服务（express）**和 **Socket 服务（net.Socket）**；
  - express server 负责直接提供**静态资源的服务**（打包后的资源直接被浏览器请求和解析）；
- HMR Socket Server，是一个 socket 的长连接(即时通信,qq,wx,送礼物)：
  - 长连接有一个最好的好处是**建立连接后双方可以通信**（服务器可以直接发送文件到客户端）；
  - 当服务器**监听到对应的模块发生变化**时，会生成**两个文件.json（manifest 文件）和.js 文件（update chunk）**；
  - 通过长连接，可以直接**将这两个文件主动发送给客户端**（浏览器）；
  - 浏览器**拿到两个新的文件**后，通过 HMR runtime 机制，**加载这两个文件**，并且**针对修改的模块进行更新**；

## HMR 的原理图

![](/pack/webpack/37.png)

## hotOnly、host 配置

- host 设置主机地址：
  - 默认值是 localhost；
  - 如果希望其他地方也可以访问，可以设置为 0.0.0.0；
- localhost 和 0.0.0.0 的区别：
  - localhost：本质上是一个域名，通常情况下会被解析成 127.0.0.1;
  - 127.0.0.1：回环地址(Loop Back Address)，表达的意思其实是我们主机自己发出去的包，直接被自己接收;
    - 正常的数据库包经常 应用层 - 传输层 - 网络层 - 数据链路层 - 物理层 ;
    - 而回环地址，是在网络层直接就被获取到了，是不会经常数据链路层和物理层的;
    - 比如我们监听 127.0.0.1 时，在同一个网段下的主机中，通过 ip 地址是不能访问的;
  - 0.0.0.0：监听 IPV4 上所有的地址，再根据端口找到不同的应用程序;
    - 比如我们监听 0.0.0.0 时，在同一个网段下的主机中，通过 ip 地址是可以访问的;

## port、open、compress

- port 设置监听的端口，默认情况下是 8080
- open 是否打开浏览器：
  - 默认值是 false，设置为 true 会打开浏览器；
  - 可以在配置中添加,也可以在 script 脚本中添加--open(脚本中添加的话 webpack-cli 会转换为在配置中把 open 设置为 true)
  - 也可以设置为类似于 Google Chrome 等值；

```js
open: true;
```

```json
"scripts": {
  "dev": "webpack serve --open"
},
```

- compress 是否为静态文件开启 gzip compression：
  - 默认值是 false，可以设置为 true；

![](/pack/webpack/38.png)

## Proxy

- proxy 是我们开发中非常常用的一个配置选项，它的目的设置代理来解决跨域访问的问题：

  - 比如我们的一个 api 请求是 http://localhost:8888，但是本地启动服务器的域名是 http://localhost:8000，这
    个时候发送网络请求就会出现跨域的问题；
  - 那么我们可以将请求**先发送到一个代理服务器，代理服务器和 API 服务器没有跨域的问题，就可以解决我们的跨域问题**了；

- 如何解决跨域?

  (开发阶段)

  - 配置 devServer

  (上线阶段)

  - 如果是 tomcat/express/koa 服务器的话就不存在跨域问题
  - 可以让服务器关闭跨域
  - 通过 nginx 代理

- 我们可以进行如下的设置：

  - target：表示的是代理到的目标地址

  * pathRewrite：默认情况下，我们的 /api-hy 也会被写入到 URL 中，如果希望删除，可以使用 pathRewrite；
  * secure：默认情况下不接收转发到 https 的服务器上，如果希望支持，可以设置为 false；
  * changeOrigin：它表示是否更新代理后请求的 headers 中 host 地址(一般我们都设置为 true)；

比如:使用后端在 localhost:3000 上，可以使用它来启用代理：

```js
module.exports = {
  //...
  devServer: {
    proxy: {
      "/api": "http://localhost:3000",
    },
  },
};
```

现在，对 /api/users 的请求会将请求代理到 http://localhost:3000/api/users。

如果不希望传递/api，则需要重写路径：

```js
module.exports = {
  //...
  devServer: {
    proxy: {
      "/api": {
        target: "http://localhost:3000",
        pathRewrite: { "^/api": "" },
      },
    },
  },
};
```

## resolve 模块解析

- resolve 用于设置模块如何被解析：
  - 在开发中我们会有各种各样的模块依赖，这些模块可能来自于自己编写的代码，也可能来自第三方库；
  - resolve 可以帮助 webpack 从每个 require/import 语句中，找到需要引入到合适的模块代码；
  - webpack 使用 [enhanced-resolve](https://github.com/webpack/enhanced-resolve) 来解析文件路径；
- webpack 能解析三种文件路径：
  - 绝对路径
    - 由于已经获得文件的绝对路径，因此不需要再做进一步解析
  - 相对路径
    - 在这种情况下，使用 import 或 require 的资源文件所处的目录，被认为是上下文目录；
    - 在 import/require 中给定的相对路径，会拼接此上下文路径，来生成模块的绝对路径；
  - 模块路径
    - 在 resolve.modules 中指定的所有目录检索模块；
      - 默认值是 'node_modules'，所以默认会从 node_modules 中查找文件；
    - 我们可以通过设置别名的方式来替换初识模块路径，具体后面讲解 alias 的配置；

## 确实文件还是文件夹

- 如果是一个文件：
  - 如果文件具有扩展名，则直接打包文件；
  - 否则，将使用 resolve.extensions 选项作为文件扩展名解析；
- 如果是一个文件夹：
  - 会在文件夹中根据 resolve.mainFiles 配置选项中指定的文件顺序查找；
    - resolve.mainFiles 的默认值是 ['index']；
    - 再根据 resolve.extensions 来解析扩展名；

## extensions 和 alias 配置

- extensions 是解析到文件时自动添加扩展名：
  - 默认值是 ['.wasm', '.mjs', '.js', '.json']；
  - 所以如果我们代码中想要添加加载 .vue 或者 jsx 或者 ts 等文件时，我们必须自己写上扩展名；

如果没有配置这个的话,没有不添加 vue 的后缀名,就会报错
![](/pack/webpack/39.png)

当我们配置了 extensions 的话就不会有问题了

```js
module.exports = {
  resolve: {
    extensions: ["...", ".vue"],
  },
};
```

- 另一个非常好用的功能是配置别名 alias：
  - 特别是当我们项目的目录结构比较深的时候，或者一个文件的路径可能需要 ../../../这种路径片段；
  - 我们可以给某些常见的路径起一个别名；

```js
module.exports = {
  resolve: {
    alias: {
      "@": path.resolve(__dirname, "./src"),
      js: path.resolve(__dirname, "./src/js"),
    },
  },
};
```

这样我们将代码写成这样也是没有问题的

```js
// import sum from './js/sum.js';
import sum from "@/js/sum.js";
// const { name, age, say } = require('./js/person.js');
const { name, age, say } = require("@/js/person.js");
// import { element } from './js/element';
import { element } from "js/element";
// import App from './vue/App';
import App from "@/vue/App";
```

## 如何区分开发环境

- 目前我们所有的 webpack 配置信息都是放到一个配置文件中的：webpack.config.js
  - 当配置越来越多时，这个文件会变得越来越不容易维护；
  - 并且某些配置是在开发环境需要使用的，某些配置是在生成环境需要使用的，当然某些配置是在开发和生成环
    境都会使用的；
  - 所以，我们最好对配置进行划分，方便我们维护和管理；
- 那么，在启动时如何可以区分不同的配置呢？
  - 方案一：编写两个不同的配置文件，开发和生成时，分别加载不同的配置文件即可；
  - 方式二：使用相同的一个入口配置文件，通过设置参数来区分它们；

```json
"scripts": {
  "build": "webpack --config ./config/webpack.build.config.js",
  "dev": "webpack serve --open --config ./config/webpack.dev.config"
},
```

## 入口文件解析

- 我们之前编写入口文件的规则是这样的：./src/index.js，但是如果我们的配置文件所在的位置变成了 config 目录，
  我们是否应该变成 ../src/index.js 呢？
  - 如果我们这样编写，会发现是报错的，依然要写成 ./src/index.js；
  - 这是因为入口文件其实是和另一个属性时有关的 context；
- context 的作用是用于解析入口（entry point）和加载器（loader）：
  - 官方说法：默认是当前路径（但是经过我测试，默认应该是 webpack 的启动目录）
  - 另外推荐在配置中传入一个值；

```js
module.exports = {
  // context是上一层目录
  context: path.resolve(__dirname, "../"),
  entry: "./src/main.js",
};
```

```js
module.exports = {
  // context是配置文件所在的目录
  context: path.resolve(__dirname, "./"),
  entry: "../src/main.js",
};
```

## 区分开发和生成环境配置

- 这里我们创建三个文件：
  - webpack.public.config.js
  - webpack.dev.config.js
  - webpack.build.config.js
- 具体的分离代码这里不再给出，查看 GitHub 代码

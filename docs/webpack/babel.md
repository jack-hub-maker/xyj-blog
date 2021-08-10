
## 为什么需要 babel？

- 事实上，在开发中我们很少直接去接触 babel，但是 babel 对于前端开发来说，目前是不可缺少的一部分：
  - 开发中，我们想要使用 **ES6+的语法**，想要使用 **TypeScript**，开发**React 项目**，它们**都是离不开 Babel 的**；
  - 所以，**学习 Babel** 对于我们理解代码从编写到线上的转变过程至关重要；
- 那么，Babel 到底是什么呢？
  - Babel 是一个**工具链**，主要用于旧浏览器或者环境中将 ECMAScript 2015+代码转换为向后兼容版本的
    JavaScript；
  - 包括：语法转换、源代码转换等；

## Babel 命令行使用

- babel 本身可以作为一个独立的工具（和 postcss 一样），不和 webpack 等构建工具配置来单独使用。
- 如果我们希望在命令行尝试使用 babel，需要安装如下库：
  - @babel/core：babel 的核心代码，必须安装；
  - @babel/cli：可以让我们在命令行使用 babel；

```sh
npm install @babel/cli @babel/core -D
```

- 使用 babel 来处理我们的源代码：
  - src：是源文件的目录；
  - --out-dir：指定要输出的文件夹 dist；

```sh
npx babel src --out-dir dist
```

比如我在根目录建立了一个 demo.js 的文件

```js
// demo.js

const name = "codertao";
const say = (name) => {
  console.log("你好啊" + name);
};
```

接下来就通过 babel 将 demo.js 转换成根目录下的 text.js

```sh
npx babel demo.js --out-file text.js
```

![18.png](https://img14.360buyimg.com/ddimg/jfs/t1/178426/13/18726/53371/61122e91E6f28a743/58e01f887713d12f.png)

转换后发现感觉没有任何转换

## 插件的使用

- 比如我们需要转换箭头函数，那么我们就可以使用箭头函数转换相关的插件：

```sh
npm install @babel/plugin-transform-arrow-functions -D
```

```sh
npx babel demo.js --out-file text.js --plugins=@babel/plugin-transform-arrow-functions
```

![19.png](https://img14.360buyimg.com/ddimg/jfs/t1/185403/40/18319/53443/61122e95E699f70e1/44f842c2660115cc.png)
- 查看转换后的结果：我们会发现 const 并没有转成 var
  - 这是因为 plugin-transform-arrow-functions，并没有提供这样的功能；
  - 我们需要使用 plugin-transform-block-scoping 来完成这样的功能；

```sh
npm install @babel/plugin-transform-block-scoping -D
```

```sh
npx babel demo.js --out-file text.js --plugins=@babel/plugin-transform-block-scoping,@babel/plugin-transform-arrow-functions
```

![20.png](https://img13.360buyimg.com/ddimg/jfs/t1/176942/34/18661/52656/61122e93Ed1a77595/0a17a6ce4ae0d614.png)
## Babel 的预设 preset

- 但是如果要转换的内容过多，一个个设置是比较麻烦的，我们可以使用预设（preset）：
  - 后面我们再具体来讲预设代表的含义；
- 安装@babel/preset-env 预设：

```sh
npm install @babel/preset-env -D
```

```sh
npx babel demo.js --out-file text.js --presets=@babel/preset-env
```

![21.png](https://img12.360buyimg.com/ddimg/jfs/t1/187179/34/17798/55312/61122f82Ebbbd92f0/71db9e353e520c1f.png)
:::tip
所以一般开发我们就直接使用预设就可以了
:::

## Babel 的底层原理

- babel 是如何做到将我们的一段代码（ES6、TypeScript、React）转成另外一段代码（ES5）的呢？
  - 从一种**源代码（原生语言）**转换成**另一种源代码（目标语言）**，这是什么的工作呢？
  - 就是**编译器**，事实上我们可以将 babel 看成就是一个编译器
  - Babel 编译器的作用就是**将我们的源代码**，转换成浏览器可以直接识别的**另外一段源代码**；
- Babel 也拥有编译器的工作流程：
  - 解析阶段（Parsing）
  - 转换阶段（Transformation）
  - 生成阶段（Code Generation）
- [https://github.com/jamiebuilds/the-super-tiny-compiler](https://github.com/jamiebuilds/the-super-tiny-compiler)

## Babel 编译器执行原理

- Babel 的执行阶段

![22.png](https://img13.360buyimg.com/ddimg/jfs/t1/193067/7/17544/82808/61122f81Eac8c8b94/0d066e62136ec52c.png)
- 当然，这只是一个简化版的编译器工具流程，在每个阶段又会有自己具体的工作：

![23.png](https://img12.360buyimg.com/ddimg/jfs/t1/196976/35/2480/359244/61122f82E2c7e34b7/c34fbabf3dbe6b04.png)

## babel-loader

- 在实际开发中，我们通常会在构建工具中通过配置 babel 来对其进行使用的，比如在 webpack 中。
- 那么我们就需要去安装相关的依赖：
  - 如果之前已经安装了@babel/core，那么这里不需要再次安装；

```sh
npm install babel-loader @babel/core
```

- 我们可以设置一个规则，在加载 js 文件时，使用我们的 babel：

```js
// 其他省略
module: {
  rules: [
    {
      test: /\.js$/,
      use: {
        loader: "babel-loader"
      }
    }
  ]
},
```

## 指定使用的插件

- 我们必须指定使用的插件才会生效

```js
{
  test: /\.js$/,
  use: {
    loader: "babel-loader",
    options: {
      plugins: [
        "@babel/plugin-transform-block-scoping",
        "@babel/plugin-transform-arrow-functions"
      ]
    }
  }
}
```

## babel-preset

- 如果我们一个个去安装使用插件，那么需要手动来管理大量的 babel 插件，我们可以直接给 webpack 提供一个
  preset，webpack 会根据我们的预设来加载对应的插件列表，并且将其传递给 babel。

- 比如常见的预设有三个：
  - env
  - react
  - TypeScript
- 安装 preset-env：

```sh
npm install @babel/preset-env
```

```js
{
  test: /\.js$/,
  use: {
    loader: "babel-loader",
    options: {
      presets: [
        "@babel/preset-env"
      ]
    }
  }
}
```

## Babel 的配置文件

- 像之前一样，我们可以将 babel 的配置信息放到一个独立的文件中，babel 给我们提供了两种配置文件的编写：
  - babel.config.json（或者.js，.cjs，.mjs）文件；
  - .babelrc.json（或者.babelrc，.js，.cjs，.mjs）文件；
- 它们两个有什么区别呢？目前很多的项目都采用了多包管理的方式（babel 本身、element-plus、umi 等）；
  - .babelrc.json：早期使用较多的配置方式，但是对于配置 Monorepos 项目是比较麻烦的；
  - rc(运行时编译)
  - babel.config.json（babel7）：可以直接作用于 Monorepos 项目的子包，更加推荐；

```js
module.exports = {
  presets: ["@babel/preset-env"],
};
```

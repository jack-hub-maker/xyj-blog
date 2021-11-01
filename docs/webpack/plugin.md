# 插件的使用

## 一、认识 Plugin

- Webpack 的另一个核心是 Plugin，官方有这样一段对 Plugin 的描述：
  - While loaders are used to transform certain types of modules, plugins can be leveraged to perform a
    wider range of tasks like bundle optimization, asset management and injection of environment
    variables.
- 上面表达的含义翻译过来就是：
  - Loader 是用于**特定的模块类型**进行转换；
  - Plugin 可以用于**执行更加广泛的任务**，比如打包优化、资源管理、环境变量注入等；

## 二、CleanWebpackPlugin

- 前面我们演示的过程中，每次修改了一些配置，重新打包时，都需要**手动删除 dist 文件夹**：
  - 我们可以借助于一个插件来帮助我们完成，这个插件就是**CleanWebpackPlugin**；
- 首先，我们先安装这个插件：

```sh
npm install clean-webpack-plugin -D
```

- 之后在插件中配置：

```js
const { CleanWebpackPlugin } = require("clean-webpack-plugin");

module.exports = {
  // 其他地方省略
  plugins: [new CleanWebpackPlugin()],
};
```

## 三、HtmlWebpackPlugin

- 另外还有一个不太规范的地方：
  - 我们的 HTML 文件是编写在根目录下的，而最终打包的 **dist 文件夹中是没有 index.html 文件**的。(事实上我们根目录是不需要 index.html 文件,以外这个插件内部有一个 ejs 模板会帮我们渲染出来,这涉及到源码的知识,后期阅读源码的时候再来进行讲解)
  - 在**进行项目部署**的时，必然也是需要**有对应的入口文件 index.html**；
  - 所以我们也需要**对 index.html 进行打包处理**;
- 对 HTML 进行打包处理我们可以使用另外一个插件：**HtmlWebpackPlugin**；

```sh
npm install html-webpack-plugin -D
```

```js
const HtmlWebpackPlugin = require("html-webpack-plugin");
module.exports = {
  // 其它省略
  plugins: [new HtmlWebpackPlugin()],
};
```

![9.png](https://img12.360buyimg.com/ddimg/jfs/t1/184293/13/18533/122777/61122e91E38edd6f3/665fe106dcf3c9f1.png)

## 四、生成 index.html 分析

- 我们会发现，现在自动在 dist 文件夹中，生成了一个 index.html 的文件：
  - 该文件中也自动添加了我们打包的 bundle.js 文件；
- 这个文件是如何生成的呢？
  - 默认情况下是根据**ejs 的一个模板**来生成的；
  - 在 html-webpack-plugin 的源码中，有一个 default_index.ejs 模块；

## 五、自定义 HTML 模板

- 如果我们想在自己的模块中加入一些比较特别的内容：
  - 比如添加一个 noscript 标签，在用户的 JavaScript 被关闭时，给予响应的提示；
  - 比如在开发 vue 或者 react 项目时，我们需要一个可以挂载后续组件的根标签 ；
- 这个我们需要一个属于自己的 index.html 模块：

```html
<!-- public/index.html -->
<!DOCTYPE html>
<html lang="">
  <head>
    <meta charset="utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width,initial-scale=1.0" />
    <link rel="icon" href="<%= BASE_URL %>favicon.ico" />
    <title><%= htmlWebpackPlugin.options.title %></title>
  </head>

  <body>
    <noscript>
      <strong
        >We're sorry but <%= htmlWebpackPlugin.options.title %> doesn't work
        properly without JavaScript enabled. Please enable it to
        continue.</strong
      >
    </noscript>
    <div id="app"></div>
  </body>
</html>
```

## 六、自定义模板数据填充

- 上面的代码中，会有一些类似这样的语法<% 变量 %>，这个是 EJS 模块填充数据的方式。
- 在配置 HtmlWebpackPlugin 时，我们可以添加如下配置：
  - template：指定我们要使用的模块所在的路径；
  - title：在进行 htmlWebpackPlugin.options.title 读取时，就会读到该信息；

## 七、DefinePlugin 的介绍

- 但是，这个时候编译还是会报错，因为在我们的模块中还使用到一个 **BASE_URL 的常量**：

![10.png](https://img14.360buyimg.com/ddimg/jfs/t1/191060/14/17516/41448/61122e90E9574e937/6fb20a21bffb745b.png)

- 这是因为在编译 template 模块时，有一个 BASE_URL：
  - 但是我们并没有设置过这个常量值，所以会出现没有定义的错误；
- 这个时候我们可以使用 DefinePlugin 插件；

## 八、DefinePlugin 的使用

- DefinePlugin 允许在编译时创建配置的全局常量，是一个 webpack 内置的插件（不需要单独安装）：

```js
const { DefinePlugin } = require("webpack");

module.exports = {
  // 其他省略
  plugins: [
    new DefinePlugin({
      BASE_URL: "'./'",
    }),
  ],
};
```

- 这个时候，编译 template 就可以正确的编译了，会读取到 **BASE_URL** 的值；

## 九、CopyWebpackPlugin

- 在 vue 的打包过程中，如果我们将一些文件**放到 public 的目录**下，那么这个目录会**被复制到 dist 文件夹中**。
  - 这个复制的功能，我们可以使用 CopyWebpackPlugin 来完成；
- 安装 CopyWebpackPlugin 插件：

```sh
npm install copy-webpack-plugin -D
```

- 接下来配置 CopyWebpackPlugin 即可：
  - 复制的规则在 patterns 中设置；
  - from：设置从哪一个源中开始复制；
  - to：复制到的位置，可以省略，会默认复制到打包的目录下；
  - globOptions：设置一些额外的选项，其中可以编写需要忽略的文件：
    - index.html：也不需要复制，因为我们已经通过 HtmlWebpackPlugin 完成了 index.html 的生成；

```js
const CopyWebpackPlugin = require("copy-webpack-plugin");

module.exports = {
  // 其他的省略
  plugins: [
    new CopyWebpackPlugin({
      patterns: [
        {
          from: "public",
          // 可以不写,默认会根据你上面的配置来读取
          // to: "./",
          // 忽略配置
          globOptions: {
            ignore: ["**/index.html"],
          },
        },
      ],
    }),
  ],
};
```

![11.png](https://img13.360buyimg.com/ddimg/jfs/t1/176960/30/18564/88623/61122e90Eba04631d/7ac79ab2fbc2cd96.png)
![12.png](https://img14.360buyimg.com/ddimg/jfs/t1/195342/30/17414/467245/61122e94E13c9f881/26c6aea452537c26.png)

## 十、Mode 配置

- 前面我们一直没有讲 mode。
- Mode 配置选项，可以告知 webpack 使用响应模式的内置优化：
  - 默认值是 production（什么都不设置的情况下）；
  - 可选值有：'none' | 'development' | 'production'；
- 这几个选项有什么样的区别呢?

比如我们在 element.js 文件中打印 codertao.lenght,这个肯定是要**报错**的

![13.png](https://img11.360buyimg.com/ddimg/jfs/t1/178047/36/18461/27482/61122e93Ef318531a/81305057dd330150.png)

在开发中我们进行调式遇到这样的错误我们是很难找到错误发生的位置的

有人会说诶这里不是说了吗,在 bundle.js 文件中的第一行

bundle.js 文件是被打包压缩过的,我们点进去以后是看不懂的,所以很难找到出现错误的位置

- 如果开发中想要详细找到 bug 的位置,我们可以通过 webpack 的配置的 mode

```js
// 设置模式
// development 开发阶段,会设置development
// production 准备打包上线的时候,设置production
mode: "development",
```

那么我们就可以找到 bug 出现的位置

![14.png](https://img14.360buyimg.com/ddimg/jfs/t1/191322/37/17800/82840/61122e90E6fa83d05/d0a6f71133b2fc13.png)
![15.png](https://img13.360buyimg.com/ddimg/jfs/t1/193224/28/17583/92660/61122e94Eaf07e754/353a284083c1eba7.png)

- 如果觉得还是不够详细,因为点开文件发现上面有一些打包后乱七八糟看不懂的代码
- 如果想出现错误,点击错误进入原文件进行查看的话.可以再配置一个 devtool

```js
// 设置source-map,建立js映射文件,方便调试代码和错误
devtool: 'source-map',
```

![16.png](https://img10.360buyimg.com/ddimg/jfs/t1/191527/39/17577/16017/61122e90E66760145/88dbed741662cd64.png)
![17.png](https://img11.360buyimg.com/ddimg/jfs/t1/198130/38/2512/63584/61122e94E20449617/e6d33e33a448e9e4.png)


这样如果开发中有了 bug,我们就可以很方便找到 bug 出现的位置然后进行修改

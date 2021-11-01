# 处理css

## 一、编写案例代码

- 我们创建一个 element.js
  - 通过 JavaScript 创建了一个元素，并且希望给它设置一些样式；

```js
// src/element.js
import "../css/style.css";

export function element() {
  const divEl = document.createElement("div");

  divEl.className = "title";
  divEl.innerHTML = "Hello World";

  document.body.appendChild(divEl);
}
```

```js
// src/main.js
import sum from "./js/sum.js";
const { name, age, say } = require("./js/person.js");
import { element } from "./js/element";

console.log(sum(20, 30));
console.log(name);
console.log(age);
say("sandy");
element();
```

然后进行`npm run build`进行打包

![3.png](https://img13.360buyimg.com/ddimg/jfs/t1/202821/9/685/44299/61122e90Ef1c0cae5/afba34937e227110.png)

## 二、css-loader 的使用

- 上面的错误信息告诉我们需要一个 loader 来加载这个 css 文件，但是**loader**是什么呢？
  - loader 可以用于**对模块的源代码**进行转换；
  - 我们可以将**css 文件也看成是一个模块**，我们是**通过 import 来加载这个模块**的；
  - 在加载这个模块时，**webpack 其实并不知道如何对其进行加载**，我们必须制定对应的 loader 来完成这个功能；
- 那么我们需要一个什么样的 loader 呢？
  - 对于加载 css 文件来说，我们需要一个可以读取 css 文件的 loader；
  - 这个 loader 最常用的是**css-loader**；
- css-loader 的安装：

```sh
npm install css-loader -D
```

## 三、css-loader 的使用方案

- **如何使用这个 loader 来加载 css 文件呢？有三种方式**：
  - 内联方式；
  - CLI 方式（webpack5 中不再使用）；
  - p 配置方式；
- **内联方式**：内联方式使用较少，因为不方便管理；

  - 在引入的样式前加上使用的 loader，并且使用!分割；

  ```js
  import "css-loader!../css/style.css";
  ```

- **CLI 方式**
  - 在 webpack5 的文档中已经没有了--module-bind；
  - 实际应用中也比较少使用，因为不方便管理；

## 四、loader 配置方式

- 配置方式表示的意思是在我们的 webpack.config.js 文件中写明配置信息：
  - module.rules 中允许我们配置多个 loader（因为我们也会继续使用其他的 loader，来完成其他文件的加载）；
  - 这种方式可以更好的表示 loader 的配置，也方便后期的维护，同时也让你对各个 Loader 有一个全局的概览；
- **module.rules 的配置如下：**
- rules 属性对应的值是一个数组：**[Rule]**
- 数组中存放的是一个个的 Rule，Rule 是一个对象，对象中可以设置多个属性：
  - **test 属性**：用于对 resource（资源）进行匹配的，通常会设置成正则表达式；
  - **use 属性**：对应的值时一个数组：**[UseEntry]**
    - UseEntry 是一个对象，可以通过对象的属性来设置一些其他属性
      - loader：必须有一个 loader 属性，对应的值是一个字符串；
      - options：可选的属性，值是一个字符串或者对象，值会被传入到 loader 中；
      - query：目前已经使用 options 来替代；
    - **传递字符串（如：use: [ 'style-loader' ]）是 loader 属性的简写方式（如：use: [ { loader: 'style-loader'} ]）**；
  - **loader 属性**： Rule.use: [ { loader } ] 的简写。

```js
// webpack.config.js
module: {
  rules: [
    {
      test: /\.css$/, // 正则表达式
      // loader: "css-loader"  // 这里的loader本质是语法糖

      // 完整的写法
      use: [
        // { loader: "css-loader" }
        "css-loader",
      ],
    },
  ];
}
```

## 五、认识 style-loader

- 我们已经可以通过 css-loader 来加载 css 文件了
  - 但是你会发现这个 css 在我们的代码中并**没有生效（页面没有效果**）。
- 但是你会发现这个 css 在我们的代码中并没有生效（页面没有效果）。
  - 因为 css-loader 只是**负责将.css 文件进行解析**，并不会将解析之后的**css 插入到页面**中；
  - 如果我们希望再完成插入 style 的操作，那么我们还需要另外一个 loader，就是 style-loader；
- 安装 style-loader：

```sh
npm install style-loader -D
```

## 六、配置 style-loader

- 那么我们应该如何使用 style-loader：
  - 在配置文件中，添加 style-loader；
  - 注意：因为 loader 的执行顺序是从右向左（或者说从下到上，或者说从后到前的），所以我们需要将 styleloader 写到 css-loader 的前面；
  ```js
  use: [
    // { loader: "css-loader" }
    "style-loader",
    "css-loader",
  ];
  ```
- 重新执行编译 npm run build，可以发现打包后的 css 已经生效了：
  - 当前目前我们的 css 是通过页内样式的方式添加进来的；
  - 后续我们也会讲如何将 css 抽取到单独的文件中，并且进行压缩等操作；

## 七、如何处理 less 文件？

- 在我们开发中，我们可能会使用 less、sass、stylus 的预处理器来编写 css 样式，效率会更高。
- 那么，如何可以让我们的环境支持这些预处理器呢
  - 首先我们需要确定，less、sass 等编写的 css 需要通过工具转换成普通的 css；
- 比如我们编写如下的 less 样式：

```less
// src/css/title.less
@bgColor: blue;
@textDecoration: underline;

.title {
  background-color: @bgColor;
  text-decoration: @textDecoration;
}
```

## 八、Less 工具处理

- 我们可以使用 less 工具来完成它的编译转换：

```sh
npm install less -D
```

- 执行如下命令：

```sh
npx lessc ./src/css/title.less ./src/css/title.css
```

![4.png](https://img12.360buyimg.com/ddimg/jfs/t1/188889/38/17866/69965/61122e92Ed2b8c7e0/b4468caa51c18db7.png)

## 九、less-loader 处理

- 但是在项目中我们会编写大量的 css，它们如何可以自动转换呢？
  - 这个时候我们就可以使用 less-loader，来自动使用 less 工具转换 less 到 css；

```sh
npm install less-loader -D
```

- 配置 webpack.config.js

```js
{
  test: /\.less$/,
  use: [
    "style-loader",
    "css-loader",
    "less-loader"
  ]
}
```

## 十、认识 PostCSS 工具

- 什么是 PostCSS 呢？
- PostCSS 是一个通过 JavaScript 来转换样式的工具；
- 这个工具可以帮助我们进行一些 CSS 的转换和适配，比如自动添加浏览器前缀、css 样式的重置；
- 但是实现这些功能，我们需要借助于 PostCSS 对应的插件；
- 如何使用 PostCSS 呢？主要就是两个步骤：
  - 第一步：查找 PostCSS 在构建工具中的扩展，比如 webpack 中的 postcss-loader；
  - 第二步：选择可以添加你需要的 PostCSS 相关的插件；

## 十一、命令行使用 postcss

- 当然，我们能不能也直接在终端使用 PostCSS 呢？
  - 也是可以的，但是我们需要单独安装一个工具 postcss-cli；
- 我们可以安装一下它们：postcss、postcss-cli

```sh
npm install postcss postcss-cli -D
```

- 我们编写一个需要添加前缀的 css：
  - [https://autoprefixer.github.io/](https://autoprefixer.github.io/)
  - 我们可以在上面的网站中查询一些添加 css 属性的样式；

## 十二、插件 autoprefixer

- 因为我们需要添加前缀，所以要安装 autoprefixer：

```sh
npm install autoprefixer -D
```

- 直接使用使用 postcss 工具，并且制定使用 autoprefixer

```sh
npx postcss --use autoprefixer -o post.css title.css
```

![5.png](https://img11.360buyimg.com/ddimg/jfs/t1/192075/34/17249/53407/61122e90Eacfaa74f/f9a3dc17bc903b7a.png)

## 十三、postcss-loader

- 真实开发中我们必然不会直接使用命令行工具来对 css 进行处理，而是可以借助于构建工具：
  - 在 webpack 中使用 postcss 就是使用 postcss-loader 来处理的；
- 我们来安装 postcss-loader：

```sh
npm install postcss-loader -D
```

- 我们修改加载 css 的 loader：（配置文件已经过多，给出一部分了）
  - 注意：因为 postcss 需要有对应的插件才会起效果，所以我们需要配置它的 plugin；

```js
{
  loader: "postcss-loader",
  options: {
    postcssOptions: {
      plugins: [
        require("autoprefixer")
      ]
    }
  }
}
```

## 十四、单独的 postcss 配置文件

- 当然，我们也可以将这些配置信息放到一个单独的文件中进行管理：
  - 在根目录下创建 postcss.config.js

```js
module.exports = {
  plugins: [require("autoprefixer")],
};
```

## 十五、postcss-preset-env

- 事实上，在配置 postcss-loader 时，我们配置插件并不需要使用 autoprefixer。
- 我们可以使用另外一个插件：postcss-preset-env
  - postcss-preset-env 也是一个 postcss 的插件；
  - 它可以帮助我们将一些现代的 CSS 特性，转成大多数浏览器认识的 CSS，并且会根据目标浏览器或者运行时环境
    添加所需的 polyfill；
  - 也包括会自动帮助我们添加 autoprefixer（所以相当于已经内置了 autoprefixer）；
- 首先，我们需要安装 postcss-preset-env：

```sh
npm install postcss-preset-env -D
```

- 之后，我们直接修改掉之前的 autoprefixer 即可：

```js
module.exports = {
  plugins: [
    // require("autoprefixer")
    require("postcss-preset-env"),
  ],
};
```

```js
module.exports = {
  plugins: [
    // require("autoprefixer")
    // require("postcss-preset-env")
    "postcss-preset-env",
  ],
};
```

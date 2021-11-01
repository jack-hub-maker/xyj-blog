# 处理静态资源
## 一、加载图片案例准备

- 为了演示我们项目中可以加载图片，我们需要在项目中使用图片，比较常见的使用图片的方式是两种：
  - img 元素，设置 src 属性；
  - 其他元素（比如 div），设置 background-image 的 css 属性；

```js
// src/element.js

// import 'css-loader!../css/style.css';
import "../css/style.css";
import "../css/title.less";
import "../css/image.css";
import sandy from "../images/sandy.jpg";

export function element() {
  const divEl = document.createElement("div");
  const bgEl = document.createElement("div");
  const imgEl = document.createElement("img");
  divEl.className = "title";
  divEl.innerHTML = "Hello World";
  bgEl.className = "image-bg";
  // 设置src,它不会根据你这里的路劲而找到对应的图片地址,而是直接将字符串的内容设置成src
  // 我们只能把src的地址当做模块化的方式来引用
  imgEl.src = sandy;

  document.body.appendChild(divEl);
  document.body.appendChild(bgEl);
  document.body.appendChild(imgEl);
}
```

```css
/* src/images/image.css */
.image-bg {
  background-image: url("../images/sandy.jpg");
  width: 200px;
  height: 200px;
  background-size: contain;
}
```

![6.png](https://img10.360buyimg.com/ddimg/jfs/t1/181027/14/18561/53976/61122e90E1789e4ca/38562bee0b7c74b9.png)

## 二、file-loader

- 要处理 jpg、png 等格式的图片，我们也需要有对应的 loader：file-loader
  - file-loader 的作用就是帮助我们处理 import/require()方式引入的一个文件资源，并且会将它放到我们输出的文
    件夹中；
  - 当然我们待会儿可以学习如何修改它的名字和所在文件夹；
- 安装 file-loader：

```sh
npm install file-loader -D
```

- 配置处理图片的 Rule：

```js
{
  test: /\.(png|jpe?g|gif|svg)$/i,
  loader: "file-loader"
}
```

## 三、文件的命名规则

- 有时候我们处理后的**文件名称**按照一定的规则进行显示：
  - 比如保留原来的**文件名**、**扩展名**，同时为了防止重复，包含一个**hash 值**等；
- 这个时候我们可以使用**PlaceHolders**来完成，webpack 给我们提供了大量的 PlaceHolders 来显示不同的内容：
  - [https://webpack.docschina.org/loaders/file-loader/](https://webpack.docschina.org/loaders/file-loader/)
  - 我们可以在文档中查阅自己需要的 placeholder；
- 我们这里介绍几个最常用的 placeholder：
  - [ext]： 处理文件的扩展名；
  - [name]：处理文件的名称
  - [hash]：文件的内容，使用 MD4 的散列函数处理，生成的一个 128 位的 hash 值（32 个十六进制）；
  - [contentHash]：在 file-loader 中和[hash]结果是一致的（在 webpack 的一些其他地方不一样，后面会讲到）；
  - [hash]：截图 hash 的长度，默认 32 个字符太长了；
  - [path]：文件相对于 webpack 配置文件的路径；

## 四、设置文件的名称

- 那么我们可以按照如下的格式编写：

```js
{
  test: /\.(png|jpe?g|gif|svg)$/i,
  use: {
    loader: "file-loader",
    // 额外配置
    options: {
      name: "[name]_[hash:6].[ext]" // 打包后的文件名称
    }
  }
}
```

## 五、设置文件的存放路径

- 当然，我们刚才通过 img/ 已经设置了文件夹，这个也是 vue、react 脚手架中常见的设置方式：
  - 其实按照这种设置方式就可以了；
  - 当然我们也可以通过 outputPath 来设置输出的文件夹；

```js
{
  test: /\.(png|jpe?g|gif|svg)$/i,
  use: {
    loader: "file-loader",
    // 额外配置
    options: {
      outputPath: "img", // 打包到指定文件夹
      name: "[name]_[hash:6].[ext]" // 打包后的文件名称
    }
  }
}
```

## 六、url-loader

- url-loader 和 file-loader 的工作方式是相似的，但是可以将较小的文件，转成 base64 的 URI。
- 安装 url-loader：

```sh
npm install url-loader -D
```

```js
{
  test: /\.(png|jpe?g|gif|svg)$/i,
  use: {
    loader: 'url-loader',
    options: {
      name: "img/[name]_[hash:6].[ext]",
    }
  }
}
```

- 在 build 文件夹中，我们会看不到图片文件：
  - 这是因为我的两张图片的大小分别是 21kb 和 18kb；
  - 默认情况下 url-loader 会将所有的图片文件转成 base64 编码

## 七、url-loader 的 limit

- 但是开发中我们往往是**小的图片需要转换**，但是**大的图片直接使用图片**即可
  - 这是因为**小的图片转换 base64**之后可以**和页面一起被请求**，**减少不必要的请求过程**；
  - 而**大的图片也进行转换**，反而会**影响页面的请求速度**；
- 那么，我们如何可以限制哪些大小的图片转换和不转换呢？

  - url-loader 有一个 options 属性**limit**，可以用于设置转换的限制；
  - 下面的代码 18kb 的图片会进行 base64 编码，而 21kb 的不会；

  ```js
  {
    test: /\.(png|jpe?g|gif|svg)$/i,
    use: {
      loader: 'url-loader',
      options: {
        name: "img/[name]_[hash:6].[ext]",
        limit: 20 * 1024  // 限制:比20kb小的图片转换成url
      }
    }
  },
  ```

![7.png](https://img10.360buyimg.com/ddimg/jfs/t1/193995/39/17520/63946/61122e93Ebd04bf49/8c6828cdb840fe1d.png)

## 八、认识 asset module type

- 我们当前使用的 webpack 版本是 webpack5：
  - 在 webpack5 之前，加载这些资源我们需要**使用一些 loader，比如 raw-loader 、url-loader、file-loader**；
  - 在 webpack5 开始，我们可以直接使用**资源模块类型（asset module type）**，来替代上面的这些 loader；
- 资源模块类型(asset module type)，通过添加 4 种新的模块类型，来替换所有这些 loader：
  - asset/resource 发送一个单独的文件并导出 URL。之前通过使用 file-loader 实现；
  - asset/inline 导出一个资源的 data URI。之前通过使用 url-loader 实现；
  - asset/source 导出资源的源代码。之前通过使用 raw-loader 实现；
  - asset 在导出一个 data URI 和发送一个单独的文件之间自动选择。之前通过使用 url-loader，并且配置资源体
    积限制实现；

## 九、asset module type 的使用

- 比如加载图片，我们可以使用下面的方式：

```js
{
  test: /\.(png|jpe?g|gif|svg)$/i,
  type: "asset",
},
```

- 但是，如何可以自定义文件的输出路径和文件名呢？
  - 方式一：修改 output，添加 assetModuleFilename 属性；(**用的很少**)
  - 方式二：在 Rule 中，添加一个 generator 属性，并且设置 filename；

```js
output: {
  path: path.resolve(__dirname, './build'),
  filename: "bundle.js",
  assetModuleFilename: "img/[name]_[hash:6][ext]",
},
```

```js
{
  test: /\.(png|jpe?g|gif|svg)$/i,
  type: "asset",
  generator: {
    filename: "img/[name]_[hash:6][ext]"
  },
}
```

## 十、url-loader 的 limit 效果

- 我们需要两个步骤来实现：
  - 步骤一：将 type 修改为 asset；
  - 步骤二：添加一个 parser 属性，并且制定 dataUrl 的条件，添加 maxSize 属性；

```js
{
  test: /\.(png|jpe?g|gif|svg)$/i,
  type: "asset",
  generator: {
    filename: "img/[name]_[hash:6][ext]"
  },
  parser: {
    dataUrlCondition: {
      maxSize: 20 * 1024
    }
  }
}
```

## 十一、加载字体文件

- 如果我们需要使用某些特殊的字体或者字体图标，那么我们会引入很多字体相关的文件，这些文件的处理也是一样
  的。

- 首先，我从阿里图标库中下载了几个字体图标：

![8.png](https://img12.360buyimg.com/ddimg/jfs/t1/178177/10/18387/148169/61122e92Ee84e4199/b41bd12f1bd278d2.png)

- 在 element.js 文件中引入，并且添加一个 i 元素用于显示字体图标：

```js
// src/element.js

import "../font/iconfont.css";

export function element() {
  const iEl = document.createElement("i");
  iEl.className = "iconfont icon-rili";
  document.body.appendChild(iEl);
}
```

## 十二、字体的打包

- 这个时候打包会报错，因为无法正确的处理 eot、ttf、woff 等文件：
  - 我们可以选择使用 file-loader 来处理，也可以选择直接使用 webpack5 的资源模块类型来处理；

```js
{
  test: /\.(woff2?|eot|ttf)$/,
  type: "asset/resource",
  generator: {
    filename: "font/[name].[hash:6][ext]"
  }
}
```

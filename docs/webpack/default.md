# 邂逅webpack
## 一、Webpack 的默认打包

我们可以通过 webpack 进行打包，之后运行打包之后的代码

在目录下直接执行 webpack 命令

```babash
webpack
```
**生成一个 dist 文件夹，里面存放一个 main.js 的文件，就是我们打包之后的文件**：
- 这个文件中的代码被压缩和丑化了；
- 另外我们发现代码中依然存在 ES6 的语法，比如箭头函数、const 等，这是因为默认情况下 webpack 并不清楚我们打包后的文
件是否需要转成 ES5 之前的语法，后续我们需要通过 babel 来进行转换和设置；
- 我们发现是可以正常进行打包的，但是有一个问题，webpack 是如何确定我们的入口的呢？
  - 事实上，当我们运行 webpack 时，webpack 会查找当前目录下的 src/index.js 作为入口；
  - 所以，如果当前项目中没有存在 src/index.js 文件，那么会报错；

**当然，我们也可以通过配置来指定入口和出口**

  ```babash
  npx webpack --entry ./src/main.js --output-path ./build
  ```

## 二、创建局部的 webpack

前面我们直接执行 webpack 命令使用的是全局的 webpack，如果希望使用局部的可以按照下面的步骤来操作。

第一步：创建 package.json 文件，用于管理项目的信息、库依赖等

  ```babash
  npm init
  ```

第二步：安装局部的 webpack
  ```bash
  npm install webpack webpack-cli -D
  ```
第三步：使用局部的 webpack
  ```bash
  npx webpack
  ```

第四步：在 package.json 中创建 scripts 脚本，执行脚本打包即可
  ```json
  "scripts": {
    "build": "webpack"
  }
  ```

以后我们打包项目就可以直接通过**npm run build** 来进行打包

## 三、Webpack 配置文件

在通常情况下，webpack 需要打包的项目是非常复杂的，并且我们需要一系列的配置来满足要求，默认配置必然
  是不可以的。

我们可以在根目录下创建一个 webpack.config.js 文件，来作为 webpack 的配置文件：

```html
<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>

  <body>
    <!-- <script src="./src/index.js"></script> -->
    <script src="./build/bundle.js"></script>
  </body>
</html>
```

```js
// src/js/person.js
const name = "xyj";
let age = 19;
const say = (name) => {
  console.log("你好啊" + name);
};
module.exports = {
  name,
  age,
  say,
};
```

```js
// src/js/sum.js
export default function sum(num1, num2) {
  return num1 + num2;
}
```

```js
// src/main.js
import sum from "./js/sum.js";
const { name, age, say } = require("./js/person.js");

console.log(sum(20, 30));
console.log(name);
console.log(age);
say("sandy");
```

```js
// webpack.config.js
const path = require("path");

module.exports = {
  // 入口
  entry: "./src/main.js",
  // 出口
  output: {
    // 出口文件夹名称
    path: path.resolve(__dirname, "./build"),
    // 出口文件名称
    filename: "bundle.js",
  },
};
```

## 四、指定配置文件

但是如果我们的配置文件并不是 webpack.config.js 的名字，而是其他的名字呢？
  - 比如我们将 webpack.config.js 修改成了 wk.config.js；
  - 这个时候我们可以通过 --config 来指定对应的配置文件；

```bash
webpack --config t.config.js
```
但是每次这样执行命令来对源码进行编译，会非常繁琐，所以我们可以在 package.json 中增加一个新的脚本：

```json
"scripts": {
  "build": "webpack --config t.config.js"
},
```

!>但是在开发中不推荐更改 webpac 配置文件的名称

## 五、Webpack 的依赖图

webpack 到底是如何对我们的项目进行打包的呢？
  - 事实上 webpack 在处理应用程序时，它会根据命令或者配置文件找到入口文件；
  - 从入口开始，会生成一个 **依赖关系图**，这个**依赖关系图**会包含应用程序中所需的所有模块（比如.js 文件、css 文件、图片、字
    体等）；
  - 然后遍历图结构，打包一个个模块（根据文件的不同使用不同的 loader 来解析）；

![2.png](https://img11.360buyimg.com/ddimg/jfs/t1/186387/27/17787/70827/61122e92E1cf3dd96/d2ae782caa05c06b.png)

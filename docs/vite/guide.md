## 浏览器原生支持模块化

测试的时候打开 index.html 必须使用 live server 打开(vscode),否则将会**跨域**

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
    <script src="./src/app.js" type="module"></script>
  </body>
</html>
```

```js
// js/foo.js
export const sum = (num1, num2) => {
  return num1 + num2;
};
```

安装一下模块来测试一下

```sh
npm install lodash-es
```

```js
// app.js
import { sum } from "./js/foo.js";
// 不能在原生模块化中这样导入，以前可以是因为webpack会帮我们解析包
// import _ from 'lodash-es';
import _ from "../node_modules/lodash-es/lodash.default.js";

console.log("Hello Vite");
console.log(sum(10, 20));
console.log(_.join(["cba", "nba"]));
```

![1.png](https://img13.360buyimg.com/ddimg/jfs/t1/179247/39/18478/34911/61122c4eEab7309ce/5acfe685b0e10a03.png)
![2.png](https://img10.360buyimg.com/ddimg/jfs/t1/177729/13/18619/98922/61122c4fE2659c02f/83fbb2a0201b73f8.png)

- 但是如果我们不借助于其他工具，直接使用 ES Module 来开发有什么问题呢？
  - 首先，我们会发现在使用 loadash 时，加载了上百个模块的 js 代码，对于浏览器发送请求是巨大的消耗；
  - 其次，我们的代码中如果有 TypeScript、less、vue 等代码时，浏览器并不能直接识别；
- 事实上，vite 就帮助我们解决了上面的所有问题。

## Vite 的安装和使用

- 注意：Vite 本身也是依赖 Node 的，所以也需要安装好 Node 环境

  - 并且 Vite 要求 Node 版本是大于 12 版本的

- 首先，我们安装一下 vite 工具：

  ```sh
  npm install vite –g # 全局安装
  npm install vite –D # 局部安装
  ```

- 通过 vite 来启动项目：

  ```sh
  npx vite
  ```

接下来就通过 Vite 来实现刚才的效果

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
    <script src="./src/app.js" type="module"></script>
  </body>
</html>
```

```js
// src/js/foo.js
export default function sum(num1, num2) {
  return num1 + num2;
}
```

```js
// src/app.js
// 发现我们这里sum导入的时候也可以不需要添加后缀名了(Vite会帮我们自动添加)
import sum from "./js/foo";
// 导入模块也会帮我们解析了
import _ from "lodash-es";

console.log("Hello Vite");
console.log(sum(20, 30));
console.log(_.join(["nba", "cba"]));
```

![3.png](https://img14.360buyimg.com/ddimg/jfs/t1/183169/16/18372/45883/61122c4eEdd573166/371b36a29be10beb.png)
![4.png](https://img13.360buyimg.com/ddimg/jfs/t1/205026/30/581/54509/61122c4eEc0899946/fd65f4fd2efeb48d.png)

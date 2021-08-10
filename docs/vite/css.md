## 处理普通的 css

- vite 可以直接支持 css 的处理
  - 直接导入 css 即可；

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

```css
/* src/css/style.css */
.title {
  font-size: 50px;
  color: red;
}
```

```js
// scc/app.js
import "./css/style.css";

const titleEl = document.createElement("div");

titleEl.className = "title";
titleEl.innerHTML = "Hello World";
document.body.appendChild(titleEl);
```

![5.png](https://img14.360buyimg.com/ddimg/jfs/t1/177948/36/18338/31857/61122c4eE1ec728e0/7439c7071154b248.png)

## 处理 css 预处理器

- vite 可以直接支持 css 预处理器，比如 less
  - 直接导入 less；
  - 之后安装 less 编译器；

```sh
npm install less -D
```

- vite 直接支持 postcss 的转换：
  - 只需要安装 postcss，并且配置 postcss.config.js 的配置文件即可；

```sh
npm install postcss postcss-preset-env -D
```

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

```less
// src/css/title.less

@fontSize: 50px;
@bgc: blue;

.title {
  font-size: @fontSize;
  background-color: @bgc;

  user-select: none;
}
```

```js
// src/app.js
import "./css/title.less";

const titleEl = document.createElement("div");

titleEl.className = "title";
titleEl.innerHTML = "Hello World";
document.body.appendChild(titleEl);
```

```js
// postcss.config.js
module.exports = {
  plugins: [require("postcss-preset-env")],
};
```

![6.png](https://img11.360buyimg.com/ddimg/jfs/t1/188060/35/17487/10877/61122c4eE4981478e/95dfe71a81e6f494.png)
![7.png](https://img13.360buyimg.com/ddimg/jfs/t1/177468/10/18622/70114/61122c4eE1512896d/87ead95d520946e6.png)

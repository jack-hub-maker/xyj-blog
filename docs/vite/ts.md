## Vite 对 TypeScript 的支持

- vite 对 TypeScript 是原生支持的，它会直接使用 ESBuild 来完成编译：
  - 只需要直接导入即可；

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

```ts
// src/ts/mult.ts

export default function mult(num1: number, num2: number): number {
  return num1 * num2;
}
```

```js
// src/app.js
import mult from "./ts/mult";

console.log(mult(20, 30));
```

![8.png](https://img14.360buyimg.com/ddimg/jfs/t1/204660/6/617/12489/61122c4eE78adb5a4/835159c13fe1cadc.png)
![9.png](https://img14.360buyimg.com/ddimg/jfs/t1/182863/38/18618/31556/61122c52Ea63250c3/2e1f6c33cde53530.png)

- 如果我们查看浏览器中的请求，会发现请求的依然是 ts 的代码:
  - 这是因为 vite 中的服务器 Connect 会对我们的请求进行转发;
  - 获取 ts 编译后的代码，给浏览器返回，浏览器可以直接进行解析；
- 注意：在 vite2 中，已经不再使用 Koa 了，而是使用 Connect 来搭建的服务器

  > 由于大多数逻辑应该通过钩子而不是中间件来完成,因此对中间件的需求大大减少,内部服务器应该用现在是一个很好的旧版的 connect 实例,而不是 Koa

> [!warning]
>**个人观点-Vite 的核心原理**
>
> Vite 会在本地建立一个 Connect 服务器,Vite 中的 Connect 服务器会对我们的请求进行转发比如这里我请求的是 foo.ts 文件,Connect 会将它转发成 foo.ts,虽然两个的>名字还是一样的,但是里面的代码被转发成了 es6 的 js 代码,不是因为 ts 才会转为 js 代码,比如你请求的 less 文件,最后也会转发成 es6 的 js 代码,然后将 js 代码返>回给浏览器让浏览器解析.第一次我们安装了新的依赖运行的时候,Vite 会帮我们把这个包给存储起来(预打包),下次我们再运行的时候,就会把上次打包过的包直接拿来用,就不需>要重新打包

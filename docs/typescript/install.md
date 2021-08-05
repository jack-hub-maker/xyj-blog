---
title: 安装
sidebarDepth: 0
---

## 安装

安装 TypeScript 非常的简单,只需要输入

```sh
npm install typescript -g
```

验证是否验证成功

```
tsc -v
```

## 使用

写一个 tsc 文件,然后在终端输入 tsc 文件名

系统就会生成一个转换后的 js 文件

然后在浏览器或者在 node 环境下执行 js 文件即可

## TypeScript 的运行环境

如果我们每次为了查看 TypeScript 代码的运行效果，都通过经过两个步骤的话就太**繁琐**了：

- 通过 tsc 编译 TypeScript 到 JavaScript 代码；
- 在浏览器或者 Node 环境下运行 JavaScript 代码；

是否可以**简化**这样的步骤呢？

- 比如编写了 TypeScript 之后可以直接运行在浏览器上？
- 比如编写了 TypeScript 之后，直接通过 node 的命令来执行？

上面我提到的两种方式，可以通过两个**解决方案**来完成：

- 通过 webpack，配置本地的 TypeScript 编译环境和开启一个本地服务，可以直接运行在浏览器上；
- 通过 ts-node 库，为 TypeScript 的运行提供执行环境；

:::tip 提示
在学习语法阶段我会选择 ts-node,在开发阶段我会选择 webpack(vite 目前还是存在很多 bug)
:::

## 使用 ts-node

安装 ts-node

```sh
npm i ts-node -g
```

另外 ts-node 需要依赖 tslib 和 @types/node 两个包：

```
npm i tslib @types/node -g
```

现在，我们可以直接通过 ts-node 来运行 TypeScript 的代码：

```js
let message: string = "Hello World";
console.log(message);

// 默认情况下ts中作用域是全局的
// 这里的export的含义是让文件是一个模块,模块在自身的作用域里执行
export {};
```

![](/type-script/2.png)

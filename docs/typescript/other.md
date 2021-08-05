---
title: 其它补充
---

## 模块化开发

TypeScript 支持两种方式来控制我们的作用域：

- 模块化：每个文件可以是一个独立的模块，支持 ES Module，也支持 CommonJS；
- 命名空间：通过 namespace 来声明一个命名空间

```ts
export function sum(num1: number, num2: number): number {
  return num1 + num2;
}
```

## 命名空间 namespace

命名空间在 TypeScript 早期时，称之为内部模块，主要目的是将一个模块内部再进行作用域的划分，防止一些命名
冲突的问题。

::: tip 闲聊
ts 是在 2014 发布的,js 的模块化是 2015(ES6)才发布的,早期 ts 没有模块化的支持,就依赖于命令空间来做类似于模块化的功能,所以命名空间也称为内部模块,这算是历史遗留问题,现在大多都使用模块化,命名空间当然也可以用
:::

```ts
export namespace Time {
  export function format(tiem: string) {}
}
```

## 类型文件

### 类型的查找

之前我们所有的 typescript 中的类型，几乎都是我们自己编写的，但是我们也有用到一些其他的类型：

```ts
const imageEl = document.querySelector("imgage") as HTMLImageElement;
```

大家是否会奇怪，我们的 HTMLImageElement 类型来自哪里呢？甚至是 document 为什么可以有 getElementById 的方
法呢？

- 其实这里就涉及到 typescript 对类型的管理和查找规则了。

我们这里先给大家介绍另外的一种 typescript 文件：.d.ts 文件

- 我们之前编写的 typescript 文件都是 .ts 文件，这些文件最终会输出 .js 文件，也是我们通常编写代码的地方；
- 还有另外一种文件 .d.ts 文件，它是用来做类型的声明(declare)。 它仅仅用来做类型检测，告知 typescript 我们有哪
  些类型；(最终.d.ts 文件不会编译成 js 文件)

那么 typescript 会在哪里查找我们的类型声明呢？

- 内置类型声明；
- 外部定义类型声明；
- 自己定义类型声明；

### 内置类型声明

内置类型声明是 typescript 自带的、帮助我们内置了 JavaScript 运行时的一些标准化 API 的声明文件；

- 包括比如 Math、Date 等内置类型，也包括 DOM API，比如 Window、Document 等；

内置类型声明通常在我们安装 typescript 的环境中会带有的；

- [https://github.com/microsoft/TypeScript/tree/main/lib](https://github.com/microsoft/TypeScript/tree/main/lib)

### 外部定义类型声明和自定义声明

外部类型声明通常是我们使用一些库（比如第三方库）时，需要的一些类型声明。

这些库通常有两种类型声明方式：

方式一：在自己库中进行类型声明（编写.d.ts 文件），比如 axios

方式一：在自己库中进行类型声明（编写.d.ts 文件），比如 axios

- 该库的 GitHub 地址：[https://github.com/DefinitelyTyped/DefinitelyTyped/](https://github.com/DefinitelyTyped/DefinitelyTyped/)
- 该库查找声明安装方式的地址：[https://www.typescriptlang.org/dt/search/?search=](https://www.typescriptlang.org/dt/search/?search=)
- 比如我们安装 react 的类型声明:我们可以在 type 中类型查找中可以找到对应需要安装的依赖

![](/type-script/17.png)

什么情况下需要自己来定义声明文件呢？

- 我们使用的第三方库是一个纯的 JavaScript 库，没有对应的声明文件；比如 lodash
- 我们给自己的代码中声明一些类型，方便在其他地方直接进行使用；

可以声明很多东西

```ts
// 声明变量
declare let name: string;

// 声明函数
declare function foo(): void;

// 声明类
declare class Person {
  name: string;
  age: number;
  constructor(name: string, age: number);
}

// 声明模块
declare module "lodash" {
  export function join(args: any): void;
}

// 声明文件
// 比如在开发vue的过程中，默认是不识别我们的.vue文件的，那么我们就需要对其进行文件的声明；
declare module "*.png";
declare module ".vue" {
  import { DefineComponent } from "vue";
  const component: DefineComponent;
}

// 声明命名空间
declare namespace time {
  function format(time: string) {}
}
```

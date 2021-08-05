---
title: 首页
sidebarDepth: 0
---

## 认识 TypeScript

虽然我们已经知道 TypeScript 是干什么的了，也知道它解决了什么样的问题，但是我们还是需要全面的来认识一下 TypeScript 到底是什么？

我们来看一下 TypeScript 在 GitHub 和官方上对自己的定义：

- GitHub 说法：TypeScript is a superset of JavaScript that compiles to clean JavaScript output.
- TypeScript 官网：TypeScript is a typed superset of JavaScript that compiles to plain JavaScript.
- 翻译一下：TypeScript 是拥有类型的 JavaScript**超集**，它可以编译成**普通、干净、完整**的 JavaScript 代码。

怎么理解上面的话呢？

- 我们可以将 TypeScript 理解成加强版的 JavaScript。
- JavaScript 所拥有的特性，TypeScript 全部都是支持的，并且它紧随 ECMAScript 的标准，所以 ES6、ES7、ES8 等新语法标准，它都是
  支持的；
- 并且在语言层面上，不仅仅增加了类型约束，而且包括一些语法的扩展，比如枚举类型（Enum）、元组类型（Tuple）等；
- TypeScript 在实现新特性的同时，总是保持和 ES 标准的同步甚至是领先；
- 并且 TypeScript 最终会被编译成 JavaScript 代码，所以你并不需要担心它的兼容性问题，在编译时也不需要借助于 Babel 这样的工具；
- 所以，我们可以把 TypeScript 理解成更加强大的 JavaScript，不仅让 JavaScript 更加安全，而且给它带来了诸多好用的好用特性；

## TypeScript 的特点

官方对 TypeScript 有几段特点的描述，我觉得非常到位（虽然有些官方，了解一下），我们一起来分享一下：

**始于** JavaScript，**归于** JavaScript

- TypeScript 从今天数以百万计的 JavaScript 开发者所熟悉的语法和语义开始。使用现有的 JavaScript 代码，包括流行的 JavaScript 库，
  并从 JavaScript 代码中调用 TypeScript 代码；
- TypeScript 可以编译出纯净、 简洁的 JavaScript 代码，并且可以运行在任何浏览器上、Node.js 环境中和任何支持 ECMAScript 3（或
  更高版本）的 JavaScript 引擎中；

TypeScript 是一个**强大**的工具，用于构建大型项目

- 类型允许 JavaScript 开发者在开发 JavaScript 应用程序时使用高效的开发工具和常用操作比如静态检查和代码重构；
- 类型是可选的，类型推断让一些类型的注释使你的代码的静态验证有很大的不同。类型让你定义软件组件之间的接口和洞察现有
  JavaScript 库的行为；

拥有**先进**的 JavaScript

- TypeScript 提供最新的和不断发展的 JavaScript 特性，包括那些来自 2015 年的 ECMAScript 和未来的提案中的特性，比如异步功能和
  Decorators，以帮助建立健壮的组件；
- 这些特性为高可信应用程序开发时是可用的，但是会被编译成简洁的 ECMAScript3（或更新版本）的 JavaScript；

## 众多项目采用 TypeScript

正是因为有这些特性，TypeScript 目前已经在很多地方被**应用**：

- Angular 源码在很早就使用 TypeScript 来进行了重写，并且开发 Angular 也需要掌握 TypeScript；
- Vue3 源码也采用了 TypeScript 进行重写，在阅读源码时会看到大量 TypeScript 的语法；
- 包括目前已经变成最流行的编辑器 VSCode 也是使用 TypeScript 来完成的；
- 包括在 React 中已经使用的 ant-design 的 UI 库，也大量使用 TypeScript 来编写；
- 目前公司非常流行 Vue3+TypeScript、React+TypeScript 的开发模式；
- 包括小程序开发，也是支持 TypeScript 的；

## 大前端的发展趋势

大前端是一群最能或者说最需要折腾的开发者：

- 客户端开发者：从 Android 到 iOS，或者从 iOS 到 Android，到 RN，甚至现在越来越多的客户端开发者接触前端
  相关知识（Vue、React、Angular、小程序）；
- 前端开发者：从 jQuery 到 AngularJS，到三大框架并行：Vue、React、Angular，还有小程序，甚至现在也要
  接触客户端开发（比如 RN、Flutter）；
- 目前又面临着不仅仅学习 ES 的特性，还要学习 TypeScript；
- 新框架的出现，我们又需要学习新框架的特性，比如 vue3.x、react18 等等；

但是每一样技术的出现都会让惊喜，因为他必然是**解决**了之前技术的某一个痛点的，而 TypeScript 真是解决了
JavaScript 存在的很多设计缺陷，尤其是关于类型检测的

并且从开发者长远的角度来看，学习 TypeScript 有助于我们前端程序员培养 类型思维，这种思维方式对于完成大
型项目尤为**重要**。

# TypeScript
## 一、JavaScript 一门优秀的语言

我始终相信：任何新技术的出现都是为了解决原有技术的某个痛点.

JavaScript 是一门优秀的编程语言吗？

- 每个人可能观点并不完全一致，但是从很多角度来看，JavaScript 是一门**非常优秀的编程语言**
- 而且，可以说在很长一段时间内**这个语言不会被代替**，并且会在**更多的领域**被大家广泛使用

著名的 Atwood 定律:

- Stack Overflow 的创立者之一的 Jeff Atwood 在 2007 年提出了著名的 Atwood 定律。
- any application that can be written in JavaScript, will eventually be written in JavaScript.
- 任何可以使用 JavaScript 来实现的应用都最终都会使用 JavaScript 实现。

其实我们已经看到了，这句话正在一步步被应验
- **Web 端**的开发我们一直都是使用 JavaScript；
- **移动端**开发可以借助于 ReactNative、Weex、Uniapp 等框架实现跨平台开发；
- **小程序端**的开发也是离不开 JavaScript；
- **桌面端**应用程序我们可以借助于 Electron 来开发；
- **服务器端**开发可以借助于 Node 环境使用 JavaScript 来开发。

## 二、JavaScript 的痛点

并且随着近几年前端领域的快速发展，让 JavaScript 迅速被**普及**和受广大开发者的**喜爱**，借助于 JavaScript 本身的
强大，也让使用 JavaScript 开发的人员越来越多。

优秀的 JavaScript 没有缺点吗？

- 其实上由于各种历史因素，JavaScript 语言本身**存在很多的缺点**；
- 比如 ES5 以及之前的使用的 var 关键字关于**作用域**的问题；
- 比如最初 JavaScript 设计的数组类型并不是连续的**内存空间**；
- 比如直到今天 JavaScript 也**没有加入**类型检测这一机制；

JavaScript 正在慢慢变好

- 不可否认的是，JavaScript 正在慢慢变得越来越好，无论是从底层设计还是应用层面.
- ES6、7、8 等的推出，每次都会让这门语言**更加现代、更加安全、更加方便**。
- 但是知道今天，JavaScript 在类型检测上依然是**毫无进展**

## 三、类型带来的问题

首先你需要知道，编程开发中我们有一个共识：**错误发现的越早越好**

- 能在写代码的时候发现错误，就不要在代码编译时再发现（IDE 的优势就是在代码编写过程中帮助我们发现错
  误）
- 能在代码编译期间发现错误，就不要在代码运行期间再发现（类型检测就可以很好的帮助我们做到这一点）
- 能在开发阶段发现错误，就不要在测试期间发现错误，能在测试期间发现错误，就不要在上线后发现错误。

现在我们想探究的就是如何在 代码编译期间 发现代码的错误：

- JavaScript 可以做到吗？不可以，我们来看下面这段经常可能出现的代码问题。

```js
function foo(message) {
  console.log(message.length);
}
foo("Hello World"); // 正确调用
foo(); // 错误调用(IDE没有报错)
```

![1.png](https://img11.360buyimg.com/ddimg/jfs/t1/186161/30/18434/66891/61122a1aE7f8d4793/7a77d7c3a1d8ded8.png)

## 四、类型错误

这是我们一个非常常见的错误：

- 这个错误很大的原因就是因为 JavaScript 没有对我们**传入的参数进行任何的限制**，只能等到**运行期间才发现这个错误**；
- 并且当这个错误产生时，会影响后续代码的继续执行，也就是整个项目都因为**一个小小的错误而深入崩溃**；
- 当然，你可能会想：我怎么可能犯这样低级的错误呢？
  - 当我们写像我们上面这样的简单的 demo 时，这样的错误很容易避免，并且当出现错误时，也很容易检查出来；
  - 但是当我们开发一个**大型项目**时呢？你能保证自己**一定不会出现这样的问题**吗？而且如果我们是调用别人的类
    库，又如何知道让我们传入的到底是什么样的参数呢？

但是，如果我们可以给 **JavaScript 加上很多限制**，在开发中就可以很好的**避免这样的问题**了：

- 比如我们的 getLength 函数中 str 是一个**必传的类型**，没有调用者没有传编译期间就会报错；
- 比如我们要求它的必须是一个 **String 类型**，传入其他类型就直接报错；
- 那么就可以知道很多的错误问题在**编译期间**就被发现，而不是等到运行时再去发现和修改；

## 五、类型思维的缺失

我们已经简单体会到没有类型检查带来的一些问题，JavaScript 因为从设计之初就没有考虑类型的约束问题，所以
造成了前端开发人员关于类型思维的缺失：

- 前端开发人员通常不关心变量或者参数是什么类型的，如果在必须确定类型时，我们往往需要使用各种判断验
  证；
- 从其他方向转到前端的人员，也会因为没有类型约束，而总是担心自己的**代码不安全，不够健壮**；

所以我们经常会说 JavaScript 不适合开发大型项目，因为当项目一旦庞大起来，这种宽松的类型约束会带来非常多
的安全隐患，多人员开发它们之间也没有良好的类型契约。

- 比如当我们去实现一个核心类库时，如果没有类型约束，那么需要对**别人传入的参数进行各种验证**来保证我们
  代码的健壮性；
- 比如我们去调用别人的函数，对方没有对函数进行任何的注释，我们只能去看里面的逻辑来理解这个函数需要
  **传入什么参数，返回值是什么类型**；

## 六、JavaScript 添加类型约束

为了弥补 JavaScript 类型约束上的缺陷，增加类型约束，很多公司推出了自己的方案：

- 2014 年，Facebook 推出了 **flow** 来对 JavaScript 进行类型检查；
- 同年，Microsoft 微软也推出了 **TypeScript1.0** 版本；
- 他们都致力于为 JavaScript 提供类型检查；

而现在，无疑 TypeScript 已经完全胜出：

- Vue2.x 的时候采用的就是 flow 来做类型检查；
- Vue3.x 已经全线转向 TypeScript，98.3%使用 TypeScript 进行了重构；
- 而 Angular 在很早期就使用 TypeScript 进行了项目重构并且需要使用 TypeScript 来进行开发；
- 而甚至 Facebook 公司一些自己的产品也在使用 TypeScript；

学习 TypeScript 不仅仅可以为我们的代码增加类型约束，而且可以培养我们前端程序员具备类型思维。

## 七、什么是TypeScript


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

## 八、TypeScript 的特点

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

## 九、众多项目采用 TypeScript

正是因为有这些特性，TypeScript 目前已经在很多地方被**应用**：

- Angular 源码在很早就使用 TypeScript 来进行了重写，并且开发 Angular 也需要掌握 TypeScript；
- Vue3 源码也采用了 TypeScript 进行重写，在阅读源码时会看到大量 TypeScript 的语法；
- 包括目前已经变成最流行的编辑器 VSCode 也是使用 TypeScript 来完成的；
- 包括在 React 中已经使用的 ant-design 的 UI 库，也大量使用 TypeScript 来编写；
- 目前公司非常流行 Vue3+TypeScript、React+TypeScript 的开发模式；
- 包括小程序开发，也是支持 TypeScript 的；

## 十、大前端的发展趋势

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

## 十一、安装TypeScript

安装 TypeScript 非常的简单,只需要输入

```sh
npm install typescript -g
```

验证是否验证成功

```
tsc -v
```

## 十二、代码转换
写一个 ts 文件,然后在终端输入 tsc 文件名

系统就会生成一个转换后的 js 文件

然后在浏览器或者在 node 环境下执行 js 文件即可

## 十三、TypeScript 的运行环境

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

## 十四、使用 ts-node

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

![2.png](https://img14.360buyimg.com/ddimg/jfs/t1/188915/28/17660/12180/61122a1aE9b7ddca5/22d3babdeb811153.png)

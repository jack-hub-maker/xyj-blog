# 浏览器的工作原理

大家是否想过当你在搜索框输入一个地址的时候，过程是怎么样的？

- 比如我输入一个 baidu.com 然后敲下回车
- baidu.com 是域名，域名会通过 DNS 服务器进行解析，解析成对应的 IP 地址
- 然后对应的服务器会返回一个 index.html 给浏览器
- 之后浏览器会对 index.html 进行解析，比如遇到 link 标签，浏览器就会去服务器上下载对应的 css 文件，遇到 script 标签就会去服务器上下载对应的 js 文件

解析是通过浏览器内核来进行的解析的

## 一、认识浏览器的内核

我们经常会说：不同的浏览器有不同的内核组成

- `Gecko`：早期被 Netscape 和 Mozilla Firefox 浏览器浏览器使用；
- `Trident`：微软开发，被 IE4~IE11 浏览器使用，但是 Edge 浏览器已经转向 Blink；
- `Webkit`：苹果基于 KHTML 开发、开源的，用于 Safari，Google Chrome 之前也在使用；
- `Blink`：是 Webkit 的一个分支，Google 开发，目前应用于 Google Chrome、Edge、Opera 等；
- 等等...

事实上，我们经常说的浏览器内核指的是浏览器的排版引擎：

排版引擎（layout engine），也称为**浏览器引擎**（browser engine）、**页面渲染引擎**（rendering engine）
或**样版引擎**

## 二、浏览器渲染过程

但是在这个执行过程中，HTML 解析的时候遇到了 JavaScript 标签，应该怎么办呢？

会停止解析 HTML，而去加载和执行 JavaScript 代码；如下图可示：

![image.png](https://img10.360buyimg.com/ddimg/jfs/t1/49180/8/16935/93950/6132e769E73f8370f/a34cd5f8debfb76f.png)

这里我用我自己的口水话来简单阐述一下过程：

<details>
<summary>查看口水话（点击展开）</summary>

首先会渲染 html 文件，因为我们第一次请求的就是 html 文件

html 无非就是很多的标签，把标签转换成对应的 html parser，然后形成 DOM 树，这个时候也可以对 DOM 进行操作，如果在 html 转成成 DOM 树的过程中有遇到 js 代码，因为 JavaScript 可以对 DOM 进行操作的，这个时候有个问题，如果 js 代码要对 DOM 树进行操作，那么谁来执行 js 代码喃，因为 JavaScript 是高级语言，CPU 是不认识 JavaScript 的，这个 js 代码其实是通过 js 引擎来执行

style 样式会转换成 css parse 然后形成 style 规则

最后 DOM 树和 style 规则结合在一起形成渲染树，这个时候其实还有一个 layout 布局，我们会通过 layout 布局再对其进行一个具体的操作

layout 这有必要吗？

当然有必要，也就是说当浏览器的状态发生变化时（比如原来我们有个元素定位在右下角，当浏览器是全屏的是正常显示的，现在我把浏览器拉的很窄，我们的元素应该也会在浏览器的右下角），所以最终我们每个元素到底放在什么位置，展示什么效果，还需要通过浏览器当前处于什么状态，再进行一个对应的 layout，最终生成最终的 render tree 然后就会进行绘制，最终浏览器将其展示

那么 js 代码由谁来执行的喃？答案就是 js 引擎

</details>


## 三、认识 JavaScript 引擎

为什么需要 JavaScript 引擎呢？

- 我们前面说过，高级的编程语言都是需要转成最终的机器指令来执行的；
- 事实上我们编写的 JavaScript 无论你交给浏览器或者 Node 执行，最后都是需要被 CPU 执行的；
- 但是 CPU 只认识自己的指令集，实际上是机器语言，才能被 CPU 所执行；
- 所以我们需要 JavaScript 引擎帮助我们将 JavaScript 代码翻译成 CPU 指令来执行；

比较常见的 JavaScript 引擎有哪些呢？

- `SpiderMonkey`：第一款 JavaScript 引擎，由 Brendan Eich 开发（也就是 JavaScript 作者）；
- `Chakra`：微软开发，用于 IE 浏览器；
- `JavaScriptCore`：WebKit 中的 JavaScript 引擎，Apple 公司开发；
- `V8`：Google 开发的强大 JavaScript 引擎，也帮助 Chrome 从众多浏览器中脱颖而出；
- 等等...

## 四、浏览器内核和 JS 引擎的关系

这里我们先以 WebKit 为例，WebKit 事实上由两部分组成的：

- WebCore：负责 HTML 解析、布局、渲染等等相关的工作；（类似于渲染层）
- JavaScriptCore：解析、执行 JavaScript 代码；（类似于逻辑层）

![image.png](https://img13.360buyimg.com/ddimg/jfs/t1/204646/21/4674/238115/613309f2Ead3c0797/204c86fde186f31d.png)

另外一个强大的 JavaScript 引擎就是 V8 引擎。

## 五、V8 引擎的原理

我们来看一下官方对 V8 引擎的定义：

- V8 是用 `C ++`编写的 `Google` 开源高性能 JavaScript 和 WebAssembly 引擎，它用于 Chrome 和 Node.js 等。
- 它实现 ECMAScript 和 WebAssembly，并在 Windows 7 或更高版本，macOS 10.12+和使用 x64，IA-32，
  ARM 或 MIPS 处理器的 Linux 系统上运行。
- V8 可以独立运行，也可以嵌入到任何 C ++应用程序中。

![image.png](https://img14.360buyimg.com/ddimg/jfs/t1/204626/33/4728/349270/61330abaE4312ac3a/86c19e5404c58e56.png)

## 六、V8 引擎的架构

V8 引擎本身的源码**非常复杂**，大概有超过 100w 行 C++代码，通过了解它的架构，我们可以知道它是如何对 JavaScript 执行的：

`Parse`模块会将 JavaScript 代码转换成 AST（抽象语法树），这是因为解释器并不直接认识 JavaScript 代码；

- 如果函数没有被调用，那么是不会被转换成 AST 的；
- Parse 的 V8 官方文档：https://v8.dev/blog/scanner

`Ignition` 是一个解释器，会将 AST 转换成 ByteCode（字节码）

- 同时会收集 TurboFan 优化所需要的信息（比如函数参数的类型信息，有了类型才能进行真实的运算）；
- 如果函数只调用一次，Ignition 会执行解释执行 ByteCode；
- Ignition 的 V8 官方文档：https://v8.dev/blog/ignition-interpreter

`TurboFan` 是一个编译器，可以将字节码编译为 CPU 可以直接执行的机器码；

- 如果一个函数被多次调用，那么就会被标记为热点函数，那么就会经过 TurboFan 转换成优化的机器码，提高代码的执行性能；
- 但是，机器码实际上也会被还原为 ByteCode，这是因为如果后续执行函数的过程中，类型发生了变化（比如 sum 函数原来执行的是
  number 类型，后来执行变成了 string 类型），之前优化的机器码并不能正确的处理运算，就会逆向的转换成字节码(反向优化)；
- TurboFan 的 V8 官方文档：https://v8.dev/blog/turbofan-jit

## 七、V8 执行的细节

那么我们的 JavaScript 源码是如何被解析（Parse 过程）的呢？

Blink 将源码交给 V8 引擎，Stream 获取到源码并且进行编码转换；

Scanner 会进行词法分析（lexical analysis），词法分析会将代码转换成 tokens；

接下来 tokens 会被转换成 AST 树，经过 Parser 和 PreParser：

- Parser 就是直接将 tokens 转成 AST 树架构；
- PreParser 称之为预解析，为什么需要预解析呢？
  - 这是因为并不是所有的 JavaScript 代码，在一开始时就会被执行。那么对所有的 JavaScript 代码进行解析，必然会
    影响网页的运行效率；
  - 所以 V8 引擎就实现了 Lazy Parsing（延迟解析）的方案，它的作用是将不必要的函数进行预解析，也就是只解析暂
    时需要的内容，而对函数的全量解析是在函数被调用时才会进行；
  - 比如我们在一个函数 outer 内部定义了另外一个函数 inner，那么 inner 函数就会进行预解析；
- 生成 AST 树后，会被 Ignition 转成字节码（bytecode），之后的过程就是代码的执行过程（后续会详细分析）。

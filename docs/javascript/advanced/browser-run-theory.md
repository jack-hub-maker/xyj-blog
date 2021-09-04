## 浏览器的工作原理

大家是否想过当你在搜索框输入一个地址的时候，过程是怎么样的？

- 比如我输入一个 baidu.com 然后敲下回车
- baidu.com 是域名，域名会通过 DNS 服务器进行解析，解析成对应的 IP 地址
- 然后对应的服务器会返回一个 index.html 给浏览器
- 之后浏览器会对 index.html 进行解析，比如遇到 link 标签，浏览器就会去服务器上下载对应的 css 文件，遇到 script 标签就会去服务器上下载对应的 js 文件

解析是通过浏览器内核来进行的解析的

## 认识浏览器的内核

我们经常会说：不同的浏览器有不同的内核组成

- `Gecko`：早期被 Netscape 和 Mozilla Firefox 浏览器浏览器使用；
- `Trident`：微软开发，被 IE4~IE11 浏览器使用，但是 Edge 浏览器已经转向 Blink；
- `Webkit`：苹果基于 KHTML 开发、开源的，用于 Safari，Google Chrome 之前也在使用；
- `Blink`：是 Webkit 的一个分支，Google 开发，目前应用于 Google Chrome、Edge、Opera 等；
- 等等...

事实上，我们经常说的浏览器内核指的是浏览器的排版引擎：

排版引擎（layout engine），也称为**浏览器引擎**（browser engine）、**页面渲染引擎**（rendering engine）
或**样版引擎**

## 浏览器渲染过程

但是在这个执行过程中，HTML 解析的时候遇到了 JavaScript 标签，应该怎么办呢？

会停止解析 HTML，而去加载和执行 JavaScript 代码；如下图可示：

![image.png](https://img10.360buyimg.com/ddimg/jfs/t1/49180/8/16935/93950/6132e769E73f8370f/a34cd5f8debfb76f.png)

这里我用我自己的口水话来简单阐述一下过程：

<details>
<summary>查看口水话（点击展开）</summary>

首先会渲染 html 文件，因为我们第一次请求的就是 html 文件

html 无非就是很多的标签，把标签转换成对应的 html parser，然后形成 DOM 树，这个时候也可以对 DOM 进行操作，如果在 html 转成成 DOM 树的过程中有遇到 js 代码，因为 JavaScript 可以对 DOM 进行操作的，这个时候有个问题，如果 js 代码要对 DOM 树进行操作，那么谁来执行 js 代码喃，因为 JavaScript 是高级语言，CPU 是不认识 JavaScript 的，这个 js 代码其实是通过 js 引擎来执行（下节讲解）

style 样式会转换成 css parse 然后形成 style 规则

最后 DOM 树和 style 规则结合在一起形成渲染树，这个时候其实还有一个 layout 布局，我们会通过 layout 布局再对其进行一个具体的操作

layout 这有必要吗？

当然有必要，也就是说当浏览器的状态发生变化时（比如原来我们有个元素定位在右下角，当浏览器是全屏的是正常显示的，现在我把浏览器拉的很窄，我们的元素应该也会在浏览器的右下角），所以最终我们每个元素到底放在什么位置，展示什么效果，还需要通过浏览器当前处于什么状态，再进行一个对应的 layout，最终生成最终的 render tree 然后就会进行绘制，最终浏览器将其展示

那么 js 代码由谁来执行的喃？答案就是 js 引擎

</details>

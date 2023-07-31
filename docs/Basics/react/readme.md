# React
## 一、React 的起源

React 是 2013 年，Facebook 开源的 JavaScript 框架，那么当时为什么 Facebook 要推出这样一款框架呢？

![image.png](https://img10.360buyimg.com/ddimg/jfs/t1/210045/38/721/115327/6142b19dE183cba0e/d857b604a2fab718.png)

这个源于一个需求，所产生的 bug：

- 该功能上线之后，总是出现 bug；
- 三个消息的数字在发生变化时，过多的操作很容易产生 bug；

bug 是否可以修复呢？当然可以修复，但是 Facebook 的工程师并不满足于此；

- 他们开始思考为什么会产生这样的问题；
- 在传统的开发模式中，我们过多的去操作界面的细节；（前端、iOS、Android）
  - 并且需要掌握和使用大量 DOM 的 API，当然我们可以通过 jQuery 来简化和适配一些 API 的使用；
- 另外关于数据（状态），往往会分散到各个地方，不方便管理和维护；

他们就去思考，是否有一种新的模式来解决上面的问题：

1.  以组件的方式去划分一个个功能模块
2.  组件内以 jsx 来描述 UI 的样子，以 state 来存储组件内的状态
3.  当应用的状态发生改变时，通过 setState 来修改状态，状态发生变化时，UI 会自动发生更新

## 二、React 的特点

### 2.1 声明式编程

声明式编程是目前整个大前端开发的模式：Vue、React、Flutter、SwiftUI；

它允许我们只需要维护自己的状态，当状态改变时，React 可以根据最新的状态去渲染我们的 UI 界面；

![image.png](https://img11.360buyimg.com/ddimg/jfs/t1/78488/2/17033/218092/6142b204E568116f5/690af293487b7b88.png)

### 2.2 组件化开发

组件化开发页面目前前端的流行趋势，我们会讲复杂的界面拆分成一个个小的组件；

如何合理的进行组件的划分和设计也是后面我会讲到的一个重点；

![](https://cn.vuejs.org/images/components.png)

### 2.3 多平台适配

2013 年，React 发布之初主要是开发 Web 页面；

2015 年，Facebook 推出了 ReactNative，用于开发移动端跨平台；（虽然目前 Flutter 非常火爆，但是还是有很多公司在使用
ReactNative）；

2017 年，Facebook 推出 ReactVR，用于开发虚拟现实 Web 应用程序；（随着 5G 的普及，VR 也会是一个火爆的应用场景）；

![image.png](https://img12.360buyimg.com/ddimg/jfs/t1/98968/5/19547/450612/6142b266Ea12e44b4/ce18b7e90d05e253.png)

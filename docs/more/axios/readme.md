## 前言

网络请求是前端中非常重要的内容,学过 `ajax` 的都知道它的重要性

目前前端中发送网络请求的方式有很多种：

1.  [Ajax](#Ajax)
2.  [jQuery-Ajax](#jQuery-Ajax)
3.  [Fetch](#Fetch)
4.  [Axios](#Axios)

这么多的方式我们究竟该选哪一个好喃

## Ajax

传统的 Ajax 是基于 XMLHttpRequest(XHR)

为什么不用它呢?

1.  非常好解释, 配置和调用方式等非常混乱
2.  编码起来看起来就非常蛋疼
3.  所以真实开发中很少直接使用, 而是使用 jQuery-Ajax

## jQuery-Ajax

在以前的前端开发中,我们我们经常会使用 jQuery-Ajax

jQuery-Ajax 相对于传统的 Ajax 非常好用

为什么不选择它呢？

1.  jQuery 整个项目太大，单纯使用 ajax 却要引入整个 JQuery 非常的不合理（采取个性化打包的方案又不能享受 CDN 服务）
2.  基于原生的 XHR 开发，XHR 本身的架构不清晰，已经有了 fetch 的替代方案
3.  尽管 JQuery 对我们前端的开发工作曾有着深远的影响，但是的确正在推出历史舞台

## Fetch

选择或者不选择它?

1.  `Fetch` 是 AJAX 的替换方案，基于 Promise 设计，很好的进行了关注分离，有很大一批人喜欢使用 fetch 进行项目开发
2.  但是 Fetch 的缺点也很明显，首先需要明确的是 Fetch 是一个 low-level（底层）的 API，没有帮助你封装好各种各样的功能
    和实现
3.  比如发送网络请求需要自己来配置 Header 的 Content-Type，不会默认携带 cookie 等
4.  比如错误处理相对麻烦（只有网络错误才会 reject，HTTP 状态码 404 或者 500 不会被标记为 reject）
5.  比如不支持取消一个请求，不能查看一个请求的进度等等

MDN Fetch 学习地址:[https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API/Using_Fetch](https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API/Using_Fetch)

## Axios

1.  axios 是目前前端使用非常广泛的网络请求库，包括 Vue 作者也是推荐在 vue 中使用 axios(很早以前)
2.  主要特点包括：在浏览器中发送 XMLHttpRequests 请求、在 node.js 中发送 http 请求、支持 Promise API、拦截请求和
    响应、转换请求和响应数据等等
3.  `axios`: ajax i/o system

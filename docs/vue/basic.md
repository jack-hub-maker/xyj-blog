## 计数器案例

首先做一个计数器的案例来邂逅 Vue

如果我们想要实现一个计数器的案例：

1.  点击+1，内容会显示数字+1
2.  点击-1，内容会显示数字-1

这里我们就来看看原生和 vue 的实现方式的不同

### JavaScript

```html
<div id="app">
  <h2 class="title"></h2>
  <button class="increment">+1</button>
  <button class="decrement">-1</button>
</div>
```

```js
const titleEl = document.querySelector(".title");
const btnInEL = document.querySelector(".increment");
const btnDeEL = document.querySelector(".decrement");

let count = 0;
titleEl.innerHTML = count;

btnInEL.addEventListener("click", () => {
  count += 1;
  titleEl.innerHTML = count;
});
btnDeEL.addEventListener("click", () => {
  count -= 1;
  titleEl.innerHTML = count;
});
```

### Vue

```html
<template>
  <div>
    <h2>{{ count }}</h2>
    <button @click="increment">+1</button>
    <button @click="decrement">-1</button>
  </div>
</template>
```

```js
export default {
  data() {
    return {
      count: 0,
    };
  },
  methods: {
    increment() {
      this.count++;
    },
    decrement() {
      this.count--;
    },
  },
};
```

### 声明式和命令式

原生开发和 Vue 开发的模式和特点，我们会发现是完全不同的，这里其实涉及到两种不同的编程范式：

1.  命令式编程和声明式编程
2.  命令式编程关注的是 “how to do”，声明式编程关注的是 “what to do”，由框架(机器)完成 “how”的过程

在原生的实现过程中，我们是如何操作的呢？

1.  我们每完成一个操作，都需要通过 JavaScript 编写一条代码，来给浏览器一个指令
2.  这样的编写代码的过程，我们称之为命令式编程
3.  在早期的原生 JavaScript 和 jQuery 开发的过程中，我们都是通过这种命令式的方式在编写代码的

在 Vue 的实现过程中，我们是如何操作的呢

1.  我们会在 createApp 传入的对象中声明需要的内容，模板 template、数据 data、方法 methods
2.  这样的编写代码的过程，我们称之为是声明式编程
3.  目前 Vue、React、Angular 的编程模式，我们称之为声明式编程

## MVVM 模型

MVC 和 MVVM 都是一种软件的体系结构

1.  MVC 是 Model – View –Controller 的简称，是在前期被使用非常框架的架构模式，比如 iOS、前端
2.  MVVM 是 Model-View-ViewModel 的简称，是目前非常流行的架构模式

通常情况下，我们也经常称 Vue 是一个 MVVM 的框架

1.  Vue 官方其实有说明，Vue 虽然并没有完全遵守 MVVM 的模型，但是整个设计是受到它的启发的
    ![Snipaste_2021-08-11_15-00-53.png](https://img12.360buyimg.com/ddimg/jfs/t1/199101/17/2648/68022/611375b4Ebc4b0aa6/46e32a6a85c279ae.png)

## template 属性

template 本身还是有一些知识点的，但是我们一般是使用 CLI 来开发的，所以我就讲一些基本的

!>templage 其实就是一个模板，让我们在模板中编写 ui 的代码

template 元素是一种用于保存客户端内容的机制，该内容再加载页面时不会被呈现，但随后可以在运行时使
用 JavaScript 实例化；

## data 属性

data 属性是传入一个函数，并且该函数需要返回一个对象：

1.  在 Vue2.x 的时候，也可以传入一个对象（虽然官方推荐是一个函数）
2.  在 Vue3.x 的时候，必须传入一个函数，否则就会直接在浏览器中报错

data 中返回的对象会被 Vue 的响应式系统劫持，之后对该对象的修改或者访问都会在劫持中被处理

1.  所以我们在 template 中通过 {{counte}} 访问 counte，可以从对象中获取到数据
2.  所以我们修改 counter 的值时，template 中的 {{counter}}也会发生改变

具体这种响应式的原理，我们后面会有专门的篇幅来讲解

## methods 属性

methods 属性是一个对象，通常我们会在这个对象中定义很多的方法

1.  这些方法可以被绑定到 template 模板中
2.  在该方法中，我们可以使用 this 关键字来直接访问到 data 中返回的对象的属性

官方文档有这么一段描述:
![Snipaste_2021-08-11_15-12-32.png](https://img14.360buyimg.com/ddimg/jfs/t1/183623/30/18800/27030/6113786fE01c20047/ae2872b643cfab49.png)

对于 this 这个知识点不是很熟悉的朋友，可以跳着到我的 js 专栏查看`占位`

## 其他属性

当然，这里还可以定义很多其他的属性，我们会在后续进行讲解：

1.  比如 props、computed、watch、emits、setup 等等
2.  也包括很多的生命周期函数


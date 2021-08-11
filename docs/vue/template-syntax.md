## 前言

React 的开发模式：

1.  React 使用的 jsx，所以对应的代码都是编写的类似于 js 的一种语法
2.  之后通过 Babel 将 jsx 编译成 React.createElement 函数调用

Vue 也支持 jsx 的开发模式（后续也会讲到）

1.  但是大多数情况下，使用基于 HTML 的模板语法
2.  在模板中，允许开发者以声明式的方式将 DOM 和底层组件实例的数据绑定在一起
3.  在底层的实现中，Vue 将模板编译成虚拟 DOM 渲染函数，这个我会在后续给大家讲到

!>所以，对于学习 Vue 来说，学习模板语法是非常重要的

## Mustache 双大括号语法

如果我们希望把数据显示到模板（template）中，使用最多的语法是 “Mustache”语法 (双大括号) 的文本插值

1.  并且我们前面提到过，data 返回的对象是有添加到 Vue 的响应式系统中
2.  当 data 中的数据发生改变时，对应的内容也会发生更新
3.  当然，Mustache 中不仅仅可以是 data 中的属性，也可以是一个 JavaScript 的**表达式**

## v-once 指令

v-once 用于指定元素或者组件只渲染一次(首次渲染)：

1.  当数据发生变化时，元素或者组件以及其所有的子元素将视为静态内容并且跳过
2.  该指令可以用于性能优化

```html
<h2 v-once>{{ count }}</h2>
<button @click="increment">+1</button>
```

这里点击按钮 count 不会发生变化

## v-text 指令

用于更新元素的 textContent

```html
<h2 v-text="count"></h2>
<!-- 等价 -->
<h2>{{count}}</h2>
```

开发中一般都会使用 Mustache 语法,v-text 用的比较少

## v-html

默认情况下，如果我们展示的内容本身是 html 的，那么 vue 并不会对其进行特殊的解析

- 如果我们希望这个内容被 Vue 可以解析出来，那么可以使用 v-html 来展示

```html
<h2 v-html="message"></h2>
```

```js
  data() {
    return {
      count: 0,
      message: "<h2>0</h2>",
    };
  },
```

在开发中 v-html 也用的比较少

## v-pre

v-pre 用于跳过元素和它的子元素的编译过程，显示原始的 Mustache 标签

- 跳过不需要编译的节点，加快编译的速度

```html
<h2 v-pre>{{count}}</h2>
```

开发中一般都会使用 Mustache 语法,v-pre 用的比较少


## v-cloak

这个指令保持在元素上直到关联组件实例结束编译
- 和 CSS 规则如 [v-cloak] { display: none } 一起用时，这个指令可以隐藏未编译的 Mustache 标签直到组件实
例准备完毕


```html
<div v-cloak>
  {{ message }}
</div>
```

```css
[v-cloak] {
  display: none;
}
```

这个其实在开发中也用的比较少

## v-bind


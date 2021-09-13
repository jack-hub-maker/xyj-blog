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
<div v-cloak>{{ message }}</div>
```

```css
[v-cloak] {
  display: none;
}
```

这个其实在开发中也用的比较少

## v-bind

前端讲的一系列指令，主要是将值插入到模板内容中

但是，除了内容需要动态来决定外，某些属性我们也希望动态来绑定

1.  比如动态绑定 a 元素的 href 属性
2.  比如动态绑定 img 元素的 src 属性

### 绑定基本属性

v-bind 用于绑定一个或多个属性值，或者向另一个组件传递 props 值(这个后面学到组件化的时候再介绍)

在开发中，有哪些属性需要动态进行绑定呢？

- 还是有很多的，比如图片的链接 src、网站的链接 href、动态绑定一些类、样式等等

v-bind 有一个对应的语法糖，也就是简写方式

在开发中，我们通常会使用语法糖的形式，因为这样更加简洁

```html
<template>
  <div>
    <!-- 完整写法 -->
    <a v-bind:href="src">跳转</a>
    <!-- 语法糖 -->
    <a :href="src">跳转</a>
    <!-- 注意和上面的区别 -->
    <a href="src">跳转</a>
  </div>
</template>
```

```js
export default {
  data() {
    return {
      src: "https://likesandy.github.io/#/",
    };
  },
};
```

### 绑定 class

在开发中，有时候我们的元素 class 也是动态的，比如：

1.  当数据为某个状态时，字体显示红色
2.  当数据另一个状态时，字体显示黑色

绑定 class 有两种方式：

1.  对象语法
2.  数组语法

#### 对象语法

我们可以传给 :class (v-bind:class 的简写) 一个对象，以动态地切换 class

```html
<template>
  <div>
    <h2 :class="person">gt</h2>
  </div>
</template>
```

```js
export default {
  data() {
    return {
      person: {
        age: 20,
      },
    };
  },
};
```

#### 数组语法

```html
<template>
  <div>
    <h2 :class="names">gt</h2>
  </div>
</template>
```

```js
export default {
  data() {
    return {
      names: ["gt", "sandy"],
    };
  },
};
```

### 绑定 style

我们可以利用 v-bind:style 来绑定一些 CSS 内联样式：

1.  这次因为某些样式我们需要根据数据动态来决定；
2.  比如某段文字的颜色，大小等等

CSS property 名可以用驼峰式 (camelCase) 或短横线分隔 (kebab-case，记得用引号括起来) 来命名

绑定 class 有两种方式：

1.  对象语法
2.  数组语法

#### 对象语法

```html
<template>
  <div>
    <h2 :style="format">gt</h2>
  </div>
</template>
```

```js
export default {
  data() {
    return {
      format: {
        color: "red",
        fontSize: "30px",
      },
    };
  },
};
```

#### 数组语法

```html
<template>
  <div>
    <h2 :style="[format1, format2]">gt</h2>
  </div>
</template>
```

```js
export default {
  data() {
    return {
      format1: {
        color: "red",
      },
      format2: {
        fontSize: "30px",
      },
    };
  },
};
```

#### 动态绑定属性

在某些情况下，我们属性的名称可能也不是固定的：

1.  前端我们无论绑定 src、href、class、style，属性名称都是固定的
2.  如果属性名称不是固定的，我们可以使用 :[属性名]=“值” 的格式来定义
3.  这种绑定的方式，我们称之为**动态绑定属性**

```html
<template>
  <div>
    <h2 :[name]="value">gt</h2>
  </div>
</template>
```

```js
export default {
  data() {
    return {
      name: "abc",
      value: "cba",
    };
  },
};
```

#### 绑定一个对象

如果我们希望将一个对象的所有属性，绑定到元素上的所有属性，应该怎么做呢？

- 非常简单，我们可以直接使用 v-bind 绑定一个 对象

案例：info 对象会被拆解成 div 的各个属性

```html
<template>
  <div>
    <h2 v-bind="info">gt</h2>
    <!-- 简写的话可读性太差 -->
    <h2 :="info">gt</h2>
  </div>
</template>
```

```js
export default {
  data() {
    return {
      info: {
        name: "gt",
        age: 19,
        height: 1.85,
      },
    };
  },
};
```

## v-on

前面我们绑定了元素的内容和属性，在前端开发中另外一个非常重要的特性就是**交互**

在前端开发中，我们需要经常和用户进行各种各样的交互：

1.  这个时候，我们就必须监听用户发生的事件，比如点击、拖拽、键盘事件等等
2.  在 Vue 中如何监听事件呢？使用 v-on 指令

接下来我们来看一下 v-on 的基本使用：

```vue
<template>
  <div id="app">
    <!-- 绑定一个表达式 -->
    <button v-on:click="count++">按钮</button>
    <!-- 绑定一个事件 -->
    <button v-on:click="btnClick">按钮</button>
    <!-- @语法糖 -->
    <button @click="btnClick">按钮</button>
    <!-- 绑定多个事件(不能使用语法糖) -->
    <button v-on="{ click: btnClick, mousemove: mousemove }">按钮</button>
  </div>
</template>
```

```js
<script>
export default {
  name: "App",
  data() {
    return {
      count: 1,
    };
  },
  methods: {
    btnClick() {
      console.log("按钮发生了点击");
    },
    mousemove() {},
  },
};
</script>
```

### v-on 参数传递

当通过 methods 中定义方法，以供@click 调用时，需要注意参数问题：

- 如果该方法不需要额外参数，那么方法后的()可以不添加。
  - 如果方法本身中有一个参数，那么会默认将原生事件 event 参数传递进去
- 如果需要同时传入某个参数，同时需要 event 时，可以通过$event 传入事件。

```vue
<template>
  <div>
    <button @click="btnClick">按钮</button>
    <button @click="btnClick2('tao', '19', $event)">按钮</button>
  </div>
</template>
```

```js
<script>
export default {
  methods: {
    btnClick(event) {
      console.log(event);
    },
    btnClick2(name, age, event) {
      console.log(name, age, event);
    },
  },
};
</script>
```

### v-on 的修饰符

v-on 支持修饰符，修饰符相当于对事件进行了一些特殊的处理：

修饰符有很多，详情查看官网：https://v3.cn.vuejs.org/api/directives.html#v-else-if

这里测试一下.right - 只当点击鼠标右键时触发。

```vue
<template>
  <div>
    <button @click.right="btnClick">按钮</button>
  </div>
</template>
```

```js
<script>
export default {
  methods: {
    btnClick() {
      console.log("你点击了鼠标右键");
    },
  },
};
</script>
```

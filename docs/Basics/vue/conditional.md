# 条件渲染

在某些情况下，我们需要根据当前的条件决定某些元素或组件是否渲染，这个时候我们就需要进行条件判断了。

Vue 提供了下面的指令来进行条件判断：

- v-if
- v-else
- v-else-if
- v-show

下面我们来对它们进行学习。

## v-if、v-else、v-else-if

v-if、v-else、v-else-if 用于根据条件来渲染某一块的内容：

- 这些内容只有在条件为 true 时，才会被渲染出来；
- 这三个指令与 JavaScript 的条件语句 if、else、else if 类似；

```vue
<template>
  <div>
    <input type="text" v-model="score" />
    <h2 v-if="score > 90">优秀</h2>
    <h2 v-else-if="score > 60">良好</h2>
    <h2 v-else>一般</h2>
  </div>
</template>
```

```js
<script>
export default {
  data() {
    return {
      score: 0,
    };
  },
  methods: {
    btnClick() {
      console.log("你点击了鼠标右键");
    },
  },
};
</script>
```

v-if 的渲染原理：

- v-if 是惰性的；
- 当条件为 false 时，其判断的内容完全不会被渲染或者会被销毁掉；
- 当条件为 true 时，才会真正渲染条件块中的内容；

## template 元素

因为 v-if 是一个指令，所以必须将其添加到一个元素上：

- 但是如果我们希望切换的是多个元素呢？
- 此时我们渲染 div，但是我们并不希望 div 这种元素被渲染；
- 这个时候，我们可以选择使用 template；

template 元素可以当做不可见的包裹元素，并且在 v-if 上使用，但是最终 template 不会被渲染出来：

```vue
<template>
  <div>
    <button @click="toggle">切换</button>
    <template v-if="isShow">
      <h2>哈哈哈</h2>
      <h2>哈哈哈</h2>
      <h2>哈哈哈</h2>
    </template>
    <template v-else>
      <h2>呵呵呵</h2>
      <h2>呵呵呵</h2>
      <h2>呵呵呵</h2>
    </template>
  </div>
</template>
```

```js
<script>
export default {
  data() {
    return {
      isShow: true,
    };
  },
  methods: {
    toggle() {
      this.isShow = !this.isShow;
    },
  },
};
</script>
```

## v-show

v-show 和 v-if 的用法看起来是一致的，也是根据一个条件决定是否显示元素或者组件：

```vue
<template>
  <div>
    <button @click="toggle">切换</button>
    <h2 v-show="isShow">哈哈哈</h2>
  </div>
</template>
```

```js
<script>
export default {
  data() {
    return {
      isShow: true,
    };
  },
  methods: {
    toggle() {
      this.isShow = !this.isShow;
    },
  },
};
</script>
```

## v-show 和 v-if 的区别

首先，在用法上的区别：

- v-show 是不支持 template；
- v-show 不可以和 v-else 一起使用；

其次，本质的区别：

- v-show 元素无论是否需要显示到浏览器上，它的 DOM 实际都是有渲染的，只是通过 CSS 的 display 属性来进行
  切换；
- v-if 当条件为 false 时，其对应的原生压根不会被渲染到 DOM 中；

开发中如何进行选择呢？

- 如果我们的原生需要在显示和隐藏之间**频繁的切换**，那么使用 v-show；
- 如果不会频繁的发生切换，那么使用 v-if；

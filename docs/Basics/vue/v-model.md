## v-model 的基本使用

表单提交是开发中非常常见的功能，也是和用户交互的重要手段：

- 比如用户在`登录、注册`时需要提交账号密码；
- 比如用户在`检索、创建、更新`信息时，需要提交一些数据；

这些都要求我们可以在代码逻辑中获取到用户提交的数据，我们通常会使用 v-model 指令来完成：

- `v-model指令`可以在表单 input、textarea 以及 select 元素上创建`双向数据绑定`；
- 它会根据`控件类型`自动选取正确的方法来更新元素；
- 尽管有些神奇，`但 v-model 本质上不过是语法糖，它负责监听用户的输入事件来更新数据`，并在某种极端场景
  下进行一些特殊处理；

```html
<template>
  <div>
    <h2>{{ message }}</h2>
    <input type="text" v-model="message" />
  </div>
</template>
```

## v-model 的原理

官方有说到，v-model 的原理其实是背后有两个操作：

- v-bind 绑定 value 属性的值；
- v-on 绑定 input 事件监听到函数中，函数会获取最新的值赋值到绑定的属性中；

```html
<input type="text" v-model="message" />
<!-- 等价于 -->
<input type="text" :value="message" @input="message = $enevt.target.value" />
```

但事实上 v-model 会在源码中会更加复杂的，后面有机会再讲

详情官网写的非常详细：https://cn.vuejs.org/v2/guide/forms.html

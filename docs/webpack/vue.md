
## Vue 源码的打包

- 安装 vue

```sh
# 因为现在是6月,默认npm导入的还是vue2的仓库,等到7月通过npm install vue安装的就是vue3的版本了
npm install vue # vue2
npm install vue@next # vue3
```

```js
import { createApp } from "vue";

const app = createApp({
  template: `<h2>{{title}}</h2>`,
  data() {
    return {
      title: "Hello World",
    };
  },
}).mount("#app");
```

- 界面上是没有效果的：
  - 并且我们查看运行的控制台，会发现如下的警告信息；

![](/pack/webpack/24.png)

这是因为 webpack 对 vue 源代码打包对其进行解析采用的不同的版本

- vue1 采用的是 runtime+compiler(会对 vue 中的 template 进行解析处理)
- vue2 采用的是 runtime-only

默认现在采用的是 vue2,所以不会对 template 进行解析处理

## Vue 打包后不同版本解析

- vue(.runtime).global(.prod).js：
  - runtime 表示不包括 compiler,相对于不会对 template 进行解析处理,包也就更小一点
  - prod 表示压缩后的版本
  - 通过浏览器中的 script src 直接使用；
  - 我们之前通过 CDN 引入和下载的 Vue 版本就是这个版本；
  - 会暴露一个全局的 Vue 来使用；
- vue(.runtime).esm-browser(.prod).js：
  - 用于通过原生 ES 模块导入使用 (在浏览器中通过 script type="module" 来使用)。
- vue(.runtime).esm-bundler.js：
  - 用于 webpack，rollup 和 parcel 等构建工具；
  - 构建工具中默认是 vue.runtime.esm-bundler.js；
  - 如果我们需要解析模板 template，那么需要手动指定 vue.esm-bundler.js；
- vue.cjs(.prod).js：
  - 服务器端渲染使用；
  - 通过 require()在 Node.js 中使用；

这就解释了为什么控制台会产生这样的警告

- 因为我们通过 webpack 打包用到的就是 vue.runtime.esm-bundler.js 的版本
- runtime 是不会编译解析 template,所以就会产生警告
  那有什么方法可以解决喃
- 可以手动在导入的时候进行修改

```js
import { createApp } from "vue/dist/vue.esm-bundler";
```

这样就没有任何问题了

![](/pack/webpack/25.png)

## 运行时+编译器 vs 仅运行时

- 在 Vue 的开发过程中我们有三种方式来编写 DOM 元素：
  - 方式一：**template 模板**的方式（之前经常使用的方式）；
  - 方式二：**render 函数**的方式，使用 h 函数来编写渲染的内容；
  - 方式三：通过 **.vue 文件** 中的 template 来编写模板；
- 它们的模板分别是如何处理的呢？
  - 方式二中的 h 函数可以直接返回一个虚拟节点，也就是 Vnode 节点；
  - 方式一和方式三的 template 都需要有**特定的代码**来对其进行解析：
    - 方式三.vue 文件中的 template 可以通过在 **vue-loader** 对其进行编译和处理；
    - 方式一种的 template 我们必须要**通过源码中一部分代码**来进行编译；
- 所以，Vue 在让我们选择版本的时候分为 运行时+编译器 vs 仅运行时
  - **运行时+编译器**包含了对 template 模板的编译代码，更加完整，但是也更大一些；
  - **仅运行时**没有包含对 template 版本的编译代码，相对更小一些;

## VSCode 对 SFC 文件的支持

- 在前面我们提到过，真实开发中多数情况下我们都是使用 SFC（ **single-file components (单文件组件)** ）。
- 我们先说一下 VSCode 对 SFC 的支持：
  - 插件一：Vetur，从 Vue2 开发就一直在使用的 VSCode 支持 Vue 的插件；
  - 插件二：Volar，官方推荐的插件（后续会基于 Volar 开发官方的 VSCode 插件）；

## 编写 App.vue 代码

- 接下来我们编写自己的 App.vue 代码

```vue
<template>
  <h2>{{ title }}</h2>
</template>

<script>
export default {
  data() {
    return {
      title: "Hello World",
    };
  },
};
</script>

<style>
h2 {
  color: red;
}
</style>
```

```js
// 这里的后缀名不能省略
import App from "./vue/App.vue";

const app = createApp(App);
app.mount("#app");
```

## App.vue 的打包过程

- 我们对代码打包会报错：我们需要合适的 Loader 来处理文件。

![](/pack/webpack/26.png)

- 这个时候我们需要使用 vue-loader：

```sh
npm install vue-loader -D #v2
npm install vue-loader@next -D #v3
```

- 在 webpack 的模板规则中进行配置：

```js
{
  test: /\.vue$/,
  loader: "vue-loader",
}
```

## @vue/compiler-sfc

![](/pack/webpack/27.png)

- 打包依然会报错，这是因为我们必须添加@vue/compiler-sfc 来对 template 进行解析：

```sh
npm install @vue/compiler-sfc -D
```

![](/pack/webpack/28.png)

- 另外我们需要配置对应的 Vue 插件：

```js
const { VueLoaderPlugin } = require("vue-loader/dist/index");
// 其余省略
plugins: [new VueLoaderPlugin()];
```

- 重新打包即可支持 App.vue 的写法
- 另外，我们也可以编写其他的.vue 文件来编写自己的组件；

![](/pack/webpack/29.png)

## 全局标识的配置

- 我们会发现控制台还有另外的一个警告：

![](/pack/webpack/30.png)

- 在 GitHub 上的文档中我们可以找到说明：

![](/pack/webpack/31.png)

- 这是两个特性的标识，一个是使用 Vue 的 Options，一个是 Production 模式下是否支持 devtools 工具；
- 虽然他们都有默认值，但是强烈建议我们手动对他们进行配置；

```js
new DefinePlugin({
  BASE_URL: "'./'",
  __VUE_OPTIONS_API__: true,
  __VUE_PROD_DEVTOOLS__: false
}),
```

![](/pack/webpack/32.png)

这个时候就没有任何问题了

最后补充一个知识,当我们用了 vue-loader 的时候,vue/compiler-sfc 会对 template 进行解析

所以在 main.js 中就可以直接这样

```js
//  import { createApp } from 'vue/dist/vue.esm-bundler';
import { createApp } from "vue";
```

也是没有任何问题的

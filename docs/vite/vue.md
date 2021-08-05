
## Vite 对 vue 的支持

- vite 对 vue 提供第一优先级支持：

  - Vue 3 单文件组件支持：[@vitejs/plugin-vue](https://github.com/vitejs/vite/tree/main/packages/plugin-vue)
  - Vue 3 JSX 支持：[@vitejs/plugin-vue-jsx](https://github.com/vitejs/vite/tree/main/packages/plugin-vue-jsx)
  - Vue 2 支持：[underfin/vite-plugin-vue2](https://github.com/underfin/vite-plugin-vue2)

- 首先要使用 vue,肯定的先按照 vue

```sh
npm install vue@next -D
```

- 安装支持 vue 的插件:

```sh
npm install @vitejs/plugin-vue -D
```

- 在 vite.config.js 中配置插件：

```js
// vite.config.js
import vue from "@vitejs/plugin-vue";

export default {
  plugins: [vue()],
};
```

```html
<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>

  <body>
    <div id="app"></div>
    <script src="./src/app.js" type="module"></script>
  </body>
</html>
```

```vue
<!-- src/vue/App.vue -->
<template>
  <h2>{{ message }}</h2>
</template>

<script>
export default {
  data() {
    return {
      message: "Hello World",
    };
  },
};
</script>
```

```js
// src/app.js
import { createApp } from "vue";
import App from "./vue/app.vue";

createApp(App).mount("#app");
```

![](/pack/vite/10.png)

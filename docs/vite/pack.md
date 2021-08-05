
## Vite 打包项目

- 我们可以直接通过 vite build 来完成对当前项目的打包工具：

```sh
npx vite build
```

- 我们可以通过 preview 的方式，开启一个本地服务来预览打包后的效果：

```sh
npx vite preview
```

为了方便,一般我们会在 package.json 文件中编写 script 中编写脚本

```json
"scripts": {
  "dev": "vite",
  "build": "vite build",
  "serve": "vite preview"
},
```

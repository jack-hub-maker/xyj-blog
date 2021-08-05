
## ESBuild 解析

- ESBuild 的特点：
  - 超快的构建速度，并且不需要缓存；
  - 支持 ES6 和 CommonJS 的模块化；
  - 支持 ES6 的 Tree Shaking；
  - 支持 Go、JavaScript 的 API；
  - 支持 TypeScript、JSX 等语法编译；
  - 支持 SourceMap；
  - 支持代码压缩；
  - 支持扩展其他插件；

## ESBuild 的构建速度

- ESBuild 的构建速度和其他构建工具速度对比：

  ![](/pack/vite/11.png)

- ESBuild 为什么这么快呢？
  - 使用 Go 语言编写的，可以直接转换成机器代码，而无需经过字节码；
  - ESBuild 可以充分利用 CPU 的多内核，尽可能让它们饱和运行；
  - ESBuild 的所有内容都是从零开始编写的，而不是使用第三方，所以从一开始就可以考虑各种性能问题；

:books:[官方文档](https://esbuild.github.io/)

# ref
## 一、Refs

在 React 的开发模式中，通常情况下不需要、也不建议直接操作 DOM 原生，但是某些特殊的情况，确实需要获取到 DOM 进行某些操作，这个时候就可以使用 Refs

详情：https://zh-hans.reactjs.org/docs/refs-and-the-dom.html

创建 refs 有三种方式

- 字符串（过时）
- 对象（官方推荐）
- 函数（老版本）


## 二、Refs 转发

Refs 默认不能应用于函数式组件，为了能够获取函数式组件中的某个 DOM 元素，这个时候就需要额外转发 Refs

详情：https://zh-hans.reactjs.org/docs/forwarding-refs.html

使用高阶组件 forwardRef 就可以对函数式组件进行一次转发，react 就会给 forwardRef 内的函数传递 ref，通过 ref 参数就可以对函数内的 DOM 元素进行绑定


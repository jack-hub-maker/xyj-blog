## 认识高阶函数

详情：https://zh-hans.reactjs.org/docs/higher-order-components.html#gatsby-focus-wrapper

高阶组件的英文是 Higher-Order Components，简称为 HOC；

官方的定义：高阶组件是参数为组件，返回值为新组件的函数；

我们可以进行如下的解析：

- 首先， 高阶组件 本身不是一个组件，而是一个函数；
- 其次，这个函数的参数是一个组件，返回值也是一个组件；

## 高阶组件的定义

高阶组件的调用过程类似于这样：

```js
const EnhancedComponent = higherOrderComponent(WrappedComponent);
```

高阶函数的编写过程类似于这样：

```js
function higherOrderComponent(WrappedComponent) {
  class EnhancedComponent extends PureComponent {
    render() {
      return <WrappedComponent />;
    }
  }
  EnhancedComponent.displayName = "newHoc";
  return EnhancedComponent;
}
```

组件的名称问题：
- 在ES6中，类表达式中类名是可以省略的；
- 组件的名称都可以通过displayName来修改；

## 高阶组件的简单应用

### 增强Props

### 登录鉴权

```js

```

### 劫持生命周期方法

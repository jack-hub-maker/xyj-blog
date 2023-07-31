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

- 在 ES6 中，类表达式中类名是可以省略的；
- 组件的名称都可以通过 displayName 来修改；

## 高阶组件的简单应用

### 增强 Props

对于一些重复的数据，我们就可以使用高阶组件拦截 props 然后添加新的 props

```js
function higherOrderComponent(WrappedComponent) {
  return (props) => {
    return <WrappedComponent {...props} addres="中国" />;
  };
}

const EnhanceSon = higherOrderComponent(Son);
```

我们多个组件都要使用 context 和函数式组件要使用的话 Class.contextType 就不通用了，为了通用性的话我们可以使用高阶组件

```js
function higherOrderComponent(WrappedComponent) {
  return (props) => {
    return (
      <UserContext.Consumer>
        {(value) => {
          return <WrappedComponent {...props} {...value} />;
        }}
      </UserContext.Consumer>
    );
  };
}

const EnhanceHome = higherOrderComponent(Home);
const EnhanceAbout = higherOrderComponent(About);
```

### 登录鉴权

如果我们想对用户的权限进行操作，有一些界面需要用户已经登录才可以使用，否则跳转到登录界面

如果我们一个一个页面进行判断代码就会非常冗余，所以也可以使用高阶组件进行优化

```js
function higherOrderComponent(WrappedComponent) {
  return (props) => {
    const { isLogin } = props;
    if (isLogin) {
      return <WrappedComponent {...props} />;
    } else {
      return <Login />;
    }
  };
}

const EnhancedComponent = higherOrderComponent(Home);
```

### 生命周期劫持

我们可以通过高阶组件，对组件的生命周期进行劫持，然后做相对于的操作

我们如果想要获取组件的渲染时间也可以使用高阶组件（因为我们不仅仅只获取一个组件的渲染时间）

```js
function higherOrderComponent(WrappedComponent) {
  return class extends PureComponent {
    render() {
      return <WrappedComponent {...this.props} />;
    }
    UNSAFE_componentWillMount() {
      this.beginTime = new Date();
    }
    componentDidMount() {
      this.endTime = new Date();
      const totalTime = this.endTime - this.beginTime;
      console.log(`组件渲染时间：${totalTime}s`);
    }
  };
}

const EnhancedComponent = higherOrderComponent(Home);
```


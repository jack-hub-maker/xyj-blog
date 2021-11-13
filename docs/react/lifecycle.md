# 生命周期
## 一、类组件的生命周期

详情：https://zh-hans.reactjs.org/docs/react-component.html

![image.png](https://img10.360buyimg.com/ddimg/jfs/t1/86446/19/19615/119215/6142b007E5e478dd1/830b950b6c6f3c8e.png)

地址：https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/

通过代码我们来看一下整体的流程（常用的生命周期）

```js
import React, { Component } from "react";

export default class Text extends Component {
  constructor() {
    super();
    this.state = {
      count: 0,
    };
    console.log("constructor执行了");
  }
  componentDidMount() {
    console.log("componentDidMount执行了");
  }
  componentDidUpdate() {
    console.log("componentDidUpdate执行了");
  }
  render() {
    console.log("render执行了");
    return (
      <div>
        <h2>当前计数：{this.state.count}</h2>
        <button onClick={(e) => this.increment()}>+1</button>
      </div>
    );
  }
  increment() {
    this.setState({
      count: this.state.count + 1,
    }); 
  }
  componentWillUnmount() {
    console.log("componentWillUnmount执行了");
  }
}
```

```js
import React, { Component } from "react";
import Text from "./test";

export default class App extends Component {
  constructor() {
    super();
    this.state = {
      isShow: true,
    };
  }
  render() {
    return (
      <div>
        <button onClick={(e) => this.btnClick()}>切换</button>
        {this.state.isShow ? <Text /> : ""}
      </div>
    );
  }
  btnClick() {
    this.setState({
      isShow: !this.state.isShow,
    });
  }
}
```

页面初始化

```
constructor执行了
render执行了
componentDidMount执行了
```

DOM 更新

```
render执行了
componentDidUpdate执行了
```

组件卸载

```
componentWillUnmount执行了
```

## 二、函数组件的生命周期

有了Hooks以后，我们就可以在hooks中模拟生命周期

```jsx
import React, { memo, useState } from "react";
import Test from "./Test";

export default memo(function App() {
  const [isShow, setIsShow] = useState(true);
  return (
    <div>
      <button onClick={(e) => setIsShow(!isShow)}>切换</button>
      {isShow && <Test />}
    </div>
  );
});
```

```jsx
import React, { memo, useState, useEffect } from "react";

export default memo(function Test() {
  const [count, setCount] = useState(0);
  useEffect(() => {
    console.log("组件发生了更新");
    return () => {
      console.log("组件被销毁了");
    };
  }, [count]);
  return (
    <div>
      <h2>当前计数：{count}</h2>
      <button onClick={(e) => setCount(count + 1)}>+1</button>
    </div>
  );
});
```

页面初始化
```
组件发生了更新
```

DOM 更新
```
组件被销毁了
组件发生了更新
```

组件卸载
```
组件被销毁了
```
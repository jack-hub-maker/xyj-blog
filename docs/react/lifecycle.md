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

这个后期学到hooks的时候再来写
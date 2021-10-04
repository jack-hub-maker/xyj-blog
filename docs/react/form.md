# 表单

## 一、受控组件

在 React 中，HTML 表单的处理方式和普通的 DOM 元素不太一样：表单元素通常会保存在一些内部的 state。

详情：https://zh-hans.reactjs.org/docs/forms.html

文档写的非常详细，我就直接来进行一个基本的演练

### 1.1 基本演练

```js
import React, { PureComponent } from "react";

export default class App extends PureComponent {
  constructor(props) {
    super(props);
    this.state = {
      username: "",
    };
  }
  render() {
    return (
      <div>
        <form onSubmit={(e) => this.handleSubmit(e)}>
          <label htmlFor="username">
            用户
            <input
              type="text"
              onChange={(e) => this.handelChange(e)}
              value={this.state.username}
            />
          </label>
          <input type="submit" value="提交" />
        </form>
      </div>
    );
  }
  handelChange(e) {
    // console.log(e.target.value);
    this.setState({ username: e.target.value });
  }
  handleSubmit(e) {
    e.preventDefault();
    console.log(this.state.username);
  }
}
```

PS：整个流程是一个`单向数据流`,而不是类似于 Vue 的双向数据绑定

首先通过 input 的 value 拿到值然后渲染出来（‘’），之后通过用户输入触发 handelChange 事件把 username 的值设置了用户输入的值，state 发生改变，render 函数重新调用，重新通过 value 拿到新的值然后进行渲染

像很多 textarea，checkbox 我就不演练了，再来演练一个 select

### 1.2 select

```js
import React, { PureComponent } from "react";

export default class App extends PureComponent {
  constructor(props) {
    super(props);
    this.state = {
      fruits: "",
    };
  }
  render() {
    return (
      <div>
        <form onSubmit={(e) => this.handleSubmit(e)}>
          <label htmlFor="fruits">
            <select
              name=""
              id=""
              onChange={(e) => this.handleChange(e)}
              value={this.state.fruits}
            >
              <option value="apple">苹果</option>
              <option value="banana">香蕉</option>
              <option value="orange">橘子</option>
            </select>
          </label>
          <input type="submit" value="提交" />
        </form>
      </div>
    );
  }
  handleChange(e) {
    this.setState({ fruits: e.target.value });
  }
  handleSubmit(e) {
    e.preventDefault();
    console.log(this.state.fruits);
  }
}
```

### 1.3 多输入

像我们需要处理很多 input 元素的时候，有可能这些逻辑是一样的，我们就可以对多个 input 元素进行一个整体的处理

我们可以给每个元素添加 name 属性，并让处理函数根据 event.target.name 的值选择要执行的操作。

```js
import React, { PureComponent } from "react";

export default class App extends PureComponent {
  constructor(props) {
    super(props);
    this.state = {
      username: "",
      password: "",
    };
  }
  render() {
    return (
      <div>
        <form onSubmit={(e) => this.handleSubmit(e)}>
          <label htmlFor="username">
            用户名
            <input
              type="text"
              name="username"
              onChange={(e) => this.handleChange(e)}
            />
          </label>
          <br />
          <label htmlFor="password">
            密码
            <input
              type="text"
              name="password"
              onChange={(e) => this.handleChange(e)}
            />
          </label>
          <br />
          <input type="submit" value="提交" />
        </form>
      </div>
    );
  }
  handleChange(e) {
    // console.log(e.target.name);
    this.setState({ [e.target.name]: e.target.value });
  }
  handleSubmit(e) {
    e.preventDefault();
    console.log(this.state.username, this.state.password);
  }
}
```

## 二、非受控组件

React 推荐大多数情况下使用 受控组件 来处理表单数据：

- 一个受控组件中，表单数据是由 React 组件来管理的；
- 另一种替代方案是使用非受控组件，这时表单数据将交由 DOM 节点来处理；

详情：https://zh-hans.reactjs.org/docs/uncontrolled-components.html

如果要使用非受控组件中的数据，那么我们需要使用 ref 来从 DOM 节点中获取表单数据。

我们还是来做一个基本的演练

```js
import React, { createRef, PureComponent } from "react";

export default class App extends PureComponent {
  constructor(props) {
    super(props);
    this.usernameRef = createRef();
  }
  render() {
    return (
      <div>
        <form onSubmit={(e) => this.handleSubmit(e)}>
          <label htmlFor="username">
            用户名
            <input type="text" ref={this.usernameRef} />
          </label>
          <input type="submit" value="提交" />
        </form>
      </div>
    );
  }
  handleSubmit(e) {
    e.preventDefault();
    console.log(this.usernameRef.current.value);
  }
}
```

!>直接操作 DOM 的话，除非一些特殊的场景，否则不推荐

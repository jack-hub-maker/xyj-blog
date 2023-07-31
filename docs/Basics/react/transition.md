# React-Transition-Group

## 一、介绍

在开发中，我们想要给一个组件的显示和消失添加某种过渡动画，可以很好的增加用户体验

当然，我们可以通过原生的 CSS 来实现这些过渡动画，但是 React 社区为我们提供了 react-transition-group 用来完成过渡动画。

React 曾为开发者提供过动画插件 react-addons-css-transition-group，后由社区维护，形成了现在的 react-transition-group。

详情：http://reactcommunity.org/react-transition-group/

这个库可以帮助我们方便的实现组件的 入场 和 离场 动画，使用时需要进行额外的安装：

react-transition-group 本身非常小，不会为我们应用程序增加过多的负担

## 二、主要组件

react-transition-group 主要包含四个组件：

- **Transition**
  - 该组件是一个和平台无关的组件（不一定要结合 CSS）；
  - 在前端开发中，我们一般是结合 CSS 来完成样式，所以比较常用的是 CSSTransition；
- **CSSTransition**
  - 在前端开发中，通常使用 CSSTransition 来完成过渡动画效果
- **SwitchTransition**
  - 两个组件显示和隐藏切换时，使用该组件
- **TransitionGroup**
  - 将多个动画组件包裹在其中，一般用于列表中元素的动画；

### 2.1 CSSTransition

CSSTransition 是基于 Transition 组件构建的：

CSSTransition 执行过程中，有三个状态：appear、enter、exit；

它们有三种状态，需要定义对应的 CSS 样式：

- 第一类，开始状态：对于的类是-appear、-enter、exit；
- 第二类：执行动画：对应的类是-appear-active、-enter-active、-exit-active；
- 第三类：执行结束：对应的类是-appear-done、-enter-done、-exit-done；

```jsx
import React, { PureComponent } from "react";
import { CSSTransition } from "react-transition-group";
import "./app.css";

export default class App extends PureComponent {
  constructor(props) {
    super(props);
    this.state = {
      isShow: true,
    };
  }
  render() {
    const { isShow } = this.state;
    return (
      <div>
        <button onClick={() => this.setState({ isShow: !isShow })}>
          显示/隐藏
        </button>
        {/* 需要进行过渡动画的元素/组件外部包裹一层CSSTransition，由CSSTransition进行控制 */}
        <CSSTransition
          // 设置过渡动画类名
          classNames="my-node"
          // 设置过渡动画时间
          timeout={200}
          // 触发进入或者退出状态
          in={isShow}
          // 退出后卸载组件
          unmountOnExit={true}
          // 是否在初次进入添加动画（需要和in同时为true）（也需要配置类名的appear样式）
          appear
          // 检测动画的执行过程的钩子函数
          onEnter={() => console.log("开始进入")}
          onEntering={() => console.log("正在进入")}
          onEntered={() => console.log("进入完成")}
          onExit={() => console.log("开始退出")}
          onExiting={() => console.log("正在退出")}
          onExited={() => console.log("退出完成")}
        >
          <h2>Hello World!</h2>
        </CSSTransition>
      </div>
    );
  }
}
```

```css
.my-node-enter,
.my-node-appear {
  opacity: 1;
  transform: scale(0.6);
}
.my-node-enter-active,
.my-node-appear-active {
  opacity: 0;
  transform: scale(1);
  transition: all 300ms;
}
.my-node-enter-done,
.my-node-appear-done {
  opacity: 1;
}
.my-node-exit {
  opacity: 1;
  transform: scale(1);
}
.my-node-exit-active {
  opacity: 0;
  transform: scale(0.6);
  transition: all 300ms;
}
.my-node-exit-done {
  opacity: 1;
}
```

### 2.2 SwitchTransition

SwitchTransition 可以完成两个组件之间切换的炫酷动画：

```jsx
import React, { PureComponent } from "react";
import { SwitchTransition, CSSTransition } from "react-transition-group";
import "./app.css";

export default class App extends PureComponent {
  constructor(props) {
    super(props);
    this.state = {
      isShow: true,
    };
  }
  render() {
    const { isShow } = this.state;
    return (
      <div style={{ textAlign: "center" }}>
        {/* SwitchTransition中主要有一个属性：mode，有两个值 */}
        {/* in-out：表示新组件先进入，旧组件再移除； */}
        {/* out-in：表示就组件先移除，新组建再进入 */}
        {/* SwitchTransition里面的CSSTransition或Transition组件不再像以前那样接受in属性来判断元素是何种状态，取而代之的是key属性*/}
        <SwitchTransition mode="out-in">
          <CSSTransition
            key={isShow ? "Hello World" : "Hello React"}
            classNames="my-node"
            timeout={300}
          >
            <button onClick={() => this.setState({ isShow: !isShow })}>
              {isShow ? "Hello World" : "Hello React"}
            </button>
          </CSSTransition>
        </SwitchTransition>
      </div>
    );
  }
}
```

```css
.my-node-enter {
  opacity: 0;
  transform: translateX(-100%);
}
.my-node-enter-active {
  opacity: 1;
  transform: translateX(0%);
  transition: all 300ms;
}
.my-node-exit {
  opacity: 1;
  transform: translateX(0%);
}
.my-node-exit-active {
  opacity: 0;
  transform: translateX(100%);
  transition: all 300ms;
}
```

### 2.3 TransitionGroup

当我们有一组动画时，需要将这些 CSSTransition 放入到一个 TransitionGroup 中来完成动画：

```jsx
import React, { PureComponent } from "react";
import { CSSTransition, TransitionGroup } from "react-transition-group";
import "./app.css";

export default class App extends PureComponent {
  constructor(props) {
    super(props);
    this.state = {
      names: ["zs", "ls", "ww", "zl"],
    };
  }
  render() {
    return (
      <div style={{ textAlign: "center" }}>
        <TransitionGroup>
          {this.state.names.map((item, index) => {
            return (
              <CSSTransition classNames="my-node" timeout={300} key={index}>
                <div>{item}</div>
              </CSSTransition>
            );
          })}
        </TransitionGroup>
        <button
          onClick={() =>
            this.setState({ names: [...this.state.names, "xyj"] })
          }
        >
          添加names
        </button>
      </div>
    );
  }
}
```

```css
.my-node-enter {
  opacity: 0;
  transform: translateX(-100%);
}
.my-node-enter-active {
  opacity: 1;
  transform: translateX(0);
  transition: all 300ms;
}
```

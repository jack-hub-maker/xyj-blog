# 跨组件的通信

## 一、Context

## 二、events

前面通过 Context 主要实现的是数据的共享，但是在开发中如果有跨组件之间的事件传递，应该如何操作呢？

- 在 Vue 中我们可以通过 Vue 的实例，快速实现一个事件总线（EventBus），来完成操作；
- 在 React 中，我们可以依赖一个使用较多的库 `events` 来完成对应的操作

详情：https://www.npmjs.com/package/events

events 常用的 API：

- 创建 EventEmitter 对象：eventBus 对象；
- 发出事件：eventBus.emit("事件名称", 参数列表);
- 监听事件：eventBus.addListener("事件名称", 监听函数)；
- 移除事件：eventBus.removeListener("事件名称", 监听函数)；

events 案例演练

```js
import React, { PureComponent } from "react";
import { EventEmitter } from "events";

const event = new EventEmitter();

class Home extends PureComponent {
  render() {
    return (
      <div>
        <button onClick={(e) => this.emitEvent()}>发出全局事件</button>
      </div>
    );
  }
  emitEvent() {
    event.emit(
      "sayHello",
      "Hello World",
      "Hello JavaScript",
      "Hello React",
      "Hello TypeScript"
    );
  }
}

export default class App extends PureComponent {
  render() {
    return (
      <div>
        <Home />
      </div>
    );
  }
  componentDidMount() {
    event.addListener("sayHello", this.receiveEvent);
  }
  componentWillUnmount() {
    event.removeListener("sayHello", this.receiveEvent);
  }
  receiveEvent(...args) {
    console.log(args);
  }
}
```

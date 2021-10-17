# Redux

## 一、了解Redux

### 1.1 JavaScript纯函数


JavaScript纯函数在JavaScript专栏已经讲过，不了解回去查看[点击跳转](../javas/../javascript/advanced/pure-function.md)

### 1.2 React中的纯函数

当然纯函数还有很多的变种，但是我们只需要理解它的核心就可以了。

为什么纯函数在函数式编程中非常重要呢？
- 因为你可以安心的写和安心的用；
- 你在写的时候保证了函数的纯度，只是但是实现自己的业务逻辑即可，不需要关心传入的内容或者依赖其他的外部变量；
- 你在用的时候，你确定你的输入内容不会被任意篡改，并且自己确定的输入，一定会有确定的输出；

React中就要求我们无论是函数还是class声明一个组件，这个组件都必须像纯函数一样，保护它们的props不被修改：

在之后学习redux中，reducer也被要求是一个纯函数。

### 1.3 为什么需要redux

JavaScript开发的应用程序，已经变得越来越复杂了：
- JavaScript需要管理的状态越来越多，越来越复杂；
- 这些状态包括服务器返回的数据、缓存数据、用户操作产生的数据等等，也包括一些UI的状态，比如某些元素是否被选中，
是否显示加载动效，当前分页；

管理不断变化的state是非常困难的：
- 状态之间相互会存在依赖，一个状态的变化会引起另一个状态的变化，View页面也有可能会引起状态的变化；
- 当应用程序复杂时，state在什么时候，因为什么原因而发生了变化，发生了怎么样的变化，会变得非常难以控制和追踪；


React是在视图层帮助我们解决了DOM的渲染过程，但是State依然是留给我们自己来管理：
- 无论是组件定义自己的state，还是组件之间的通信通过props进行传递；也包括通过Context进行数据之间的共享；
- React主要负责帮助我们管理视图，state如何维护最终还是我们自己来决定；

Redux就是一个帮助我们管理State的容器：Redux是JavaScript的状态容器，提供了可预测的状态管理；

Redux除了和React一起使用之外，它也可以和其他界面库一起来使用（比如Vue），并且它非常小（包括依赖在内，只有2kb）

## 二、Redux的核心理念

### 2.1 Store

比如我们有一个朋友列表需要管理：
- 如果我们没有定义统一的规范来操作这段数据，那么整个数据的变化就是无法跟踪的；
- 比如页面的某处通过products.push的方式增加了一条数据；
- 比如另一个页面通过products[0].age = 25修改了一条数据；

整个应用程序错综复杂，当出现bug时，很难跟踪到底哪里发生的变化；

所以Redux要求我们把state都放在store里进行管理

### 2.2 action

Redux要求我们通过action来更新数据：
- 所有数据的变化，必须通过派发（dispatch）action来更新；
- action是一个普通的JavaScript对象，用来描述这次更新的type和content；

action是一个普通的JavaScript对象，用来描述这次更新的type和content；
- 强制使用action的好处是可以清晰的知道数据到底发生了什么样的变化，所有的数据变化都是可跟追、可预测的；
- 当然，目前我们的action是固定的对象，真实应用中，我们会通过函数来定义，返回一个action；

### 2.3 reducer

但是如何将state和action联系在一起呢？答案就是reducer
- reducer是一个纯函数；
- reducer做的事情就是将传入的state和action结合起来生成一个新的state；

## 三、Redux的三大原则

`单一数据源`
- 整个应用程序的state被存储在一颗object tree中，并且这个object tree只存储在一个 store 中：
- Redux并没有强制让我们不能创建多个Store，但是那样做并不利于数据的维护；
- 单一的数据源可以让整个应用程序的state变得方便维护、追踪、修改；

`State是只读的`
- 唯一修改State的方法一定是触发action，不要试图在其他地方通过任何的方式来修改State：
- 这样就确保了View或网络请求都不能直接修改state，它们只能通过action来描述自己想要如何修改state；
- 这样可以保证所有的修改都被集中化处理，并且按照严格的顺序来执行，所以不需要担心race condition（竟态）的问题；

`使用纯函数来执行修改`
- 通过reducer将 旧state和 actions联系在一起，并且返回一个新的State：
- 随着应用程序的复杂度增加，我们可以将reducer拆分成多个小的reducers，分别操作不同state tree的一部分；
- 但是所有的reducer都应该是纯函数，不能产生任何的副作用；

## 四、Redux的基本使用

参考资料：
- http://cn.redux.js.org/（中文官网）
- https://redux.js.org/（英文官网）

Redux的使用过程
1.  创建一个对象，作为我们要保存的状态：
2.  创建Store来存储这个state
    - 创建store时必须创建reducer；
    - 我们可以通过 store.getState 来获取当前的state
3.  通过action来修改state
    - 通过dispatch来派发action；
    - 通常action中都会有type属性，也可以携带其他的数据；
4.  修改reducer中的处理代码
    - 这里一定要记住，reducer是一个纯函数，不需要直接修改state；
    - 后面我会讲到直接修改state带来的问题；
5.  可以在派发action之前，监听store的变化：

```js
const redux = require('redux')

// state
const counter = {
  count: 1,
};

// reducer
function reducer(state = counter, action) {
  switch (action.type) {
    case "INCREMENT":
      return { ...state, count: state.count + 1 };
    case "DECREMENT":
      return { ...state, count: state.count - 1 };
    case "ADD_NUMBER":
      return { ...state, count: state.count + action.num };
    case "SUB_NUMBER":
      return { ...state, count: state.count - action.num };
    default:
      return state;
  }
}

// store
const store = redux.createStore(reducer);

// 订阅store的改变
store.subscribe(() => {
  // console.log(store.getState())
  console.log("state发生了改变", store.getState().count)
});

// action
const increment = { type: "INCREMENT" };
const decrement = { type: "DECREMENT" };
const add = { type: "ADD_NUMBER", num: 5 };
const sub = { type: "SUB_NUMBER", num: 10 };

// 派发action
store.dispatch(increment);
store.dispatch(decrement);
store.dispatch(add);
store.dispatch(sub);
```

## 五、Redux结构划分

如果我们将所有的逻辑代码写到一起，那么当redux变得复杂时代码就难以维护。
- 接下来，我会对代码进行拆分，将store、reducer、action、constants拆分成一个个文件。
- 创建store/index.js文件：
- 创建store/reducer.js文件：
- 创建store/actionCreators.js文件：
- 创建store/constants.js文件：

注意：node中对ES6模块化的支持
- 目前我使用的node版本是v16.11，从node v13.2.0开始，node才对ES6模块化提供了支持：
- node v13.2.0之前，需要进行如下操作：
  - 在package.json中添加属性： "type": "module"；
  - 在执行命令中添加如下选项：node --experimental-modules src/index.js;
- node v13.2.0之后，只需要进行如下操作：
  - 在package.json中添加属性： "type": "module"；
- 注意：导入文件时，需要跟上.js后缀名；


app.js
```js
import store from "./store/store.js";
import { addAction, subAction } from "./store/action.js";

store.subscribe(() => {
  console.log("state发生了改变", store.getState().count);
});

store.dispatch(addAction(5));
store.dispatch(subAction(10));
```

store/store.js
```js
import { createStore } from "redux";
import reduer from "./reduer.js";

const store = createStore(reduer);

export default store;
```

store/reduer
```js
import { ADD_NUMBER, SUB_NUMBER } from "./action-type.js";

const initialState = {
  count: 0,
};

function reduer(state = initialState, action) {
  switch (action.type) {
    case ADD_NUMBER:
      return { ...state, count: state.count + action.num };
    case SUB_NUMBER:
      return { ...state, count: state.count - action.num };
    default:
      return state;
  }
}
export default reduer;
```

store/action
```js
import { ADD_NUMBER, SUB_NUMBER } from "./action-type.js";

export const addAction = (num) => ({
  type: ADD_NUMBER,
  num,
});
export const subAction = (num) => ({
  type: SUB_NUMBER,
  num,
});
```

store/action-type
```js
export const ADD_NUMBER = "ADD_NUMBER";
export const SUB_NUMBER = "SUB_NUMBER";
```
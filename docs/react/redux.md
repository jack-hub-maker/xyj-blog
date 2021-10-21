# Redux

!>还有很多东西没写完，暂时不写了，等待后续更新

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

## 六、react-redux

那么Redux如何合React一起使用喃？我们来简单演示一下

App.jsx
```jsx
import React from "react";
import About from "./About";
import Home from "./Home";

export default function App() {
  return (
    <div>
      <Home />
      <hr />
      <About />
    </div>
  );
}
```


Home.jsx && ABout.jsx
```jsx
import React, { PureComponent } from "react";
import store from "./store/store";
import { addAction, subAction } from "./store/action";

export default class Home extends PureComponent {
  constructor(props) {
    super(props);
    this.state = {
      count: store.getState().count,
    };
  }
  componentDidMount() {
    store.subscribe(() => {
      this.setState({
        count: store.getState().count,
      });
    });
  }
  render() {
    return (
      <div>
        <h2>About</h2>
        <h2>当前计数：{this.state.count}</h2>
        <button onClick={() => store.dispatch(addAction(5))}>+5</button>
        <button onClick={() => store.dispatch(subAction(10))}>-10</button>
      </div>
    );
  }
}
```

虽然我们能成功的展示出效果，但是你会发现代码非常的冗余，Home组件和ABout的组件几乎是一样的，这里只是两个组件用到了Redux，那么有10个，100个组件都是这样的，那我们的代码就显得非常冗余

所以我们要对它进行进一步的封装

其实我们可以封装一个connect的函数来让component和store连接在一起




开始之前需要强调一下，redux和react没有直接的关系，你完全可以在React, Angular, Ember, jQuery, or vanilla JavaScript中
使用Redux。

尽管这样说，redux依然是和React或者Deku的库结合的更好，因为他们是通过state函数来描述界面的状态，Redux可以发射状
态的更新，让他们作出相应。

虽然我们之前已经实现了connect、Provider这些帮助我们完成连接redux、react的辅助工具，但是实际上redux官方帮助我们
提供了 react-redux 的库，可以直接在项目中使用，并且实现的逻辑会更加的严谨和高效。

详情：https://cn.redux.js.org/tutorials/fundamentals/part-5-ui-react#integrating-redux-with-a-ui

[![Go to CodeSandbox](https://camo.githubusercontent.com/90808661433696bc57dce8d4ad732307b5cec6270e6b846f114dcd7ee7f9458a/68747470733a2f2f636f646573616e64626f782e696f2f7374617469632f696d672f706c61792d636f646573616e64626f782e737667)](https://codesandbox.io/embed/awesome-violet-z5chr?fontsize=14&hidenavigation=1&theme=dark)

## 八、组件中异步操作

在之前简单的案例中，redux中保存的counter是一个本地定义的数据
- 我们可以直接通过同步的操作来dispatch action，state就会被立即更新。
- 但是真实开发中，redux中保存的很多数据可能来自服务器，我们需要进行异步的请求，再将数据保存到redux中。

在之前学习网络请求的时候我们讲过，网络请求可以在class组件的componentDidMount中发送，所以我们可以有这样的结构：

我现在完成如下案例操作：
- 在Home组件中请求recommends的数据；
- 在About组件中展示recommends的数据；

[![Go to CodeSandbox](https://camo.githubusercontent.com/90808661433696bc57dce8d4ad732307b5cec6270e6b846f114dcd7ee7f9458a/68747470733a2f2f636f646573616e64626f782e696f2f7374617469632f696d672f706c61792d636f646573616e64626f782e737667)](https://codesandbox.io/embed/elated-goodall-1c6z4?fontsize=14&hidenavigation=1&theme=dark)

## 九、redux中异步操作

上面的代码有一个缺陷：
- 我们必须将网络请求的异步代码放到组件的生命周期中来完成；
- 事实上，网络请求到的数据也属于我们状态管理的一部分，更好的一种方式应该是将其也交给redux来管理；

![](https://gitee.com/itsandy/picgo-img/raw/master/react/redux中异步操作.png)

但是在redux中如何可以进行异步的操作呢？
- 答案就是使用`中间件（Middleware）`；
- 学习过Express或Koa框架的朋友对中间件的概念一定不陌生；
- 在这类框架中，Middleware可以帮助我们在请求和响应之间嵌入一些操作的代码，比如cookie解析、日志记录、文件压缩等操作；

## 十、理解中间件

redux也引入了中间件（Middleware）的概念：
- 这个中间件的目的是在dispatch的action和最终达到的reducer之间，扩展一些自己的代码；
- 比如日志记录、调用异步接口、添加代码调试功能等等；

我们现在要做的事情就是发送异步的网络请求，所以我们可以添加对应的中间件：
- 这里官网推荐的、包括演示的网络请求的中间件是使用 redux-thunk；

详情：https://redux.js.org/tutorials/essentials/part-5-async-logic#thunks-and-async-logic

redux-thunk是如何做到让我们可以发送异步的请求呢？
- 我们知道，默认情况下的dispatch(action)，action需要是一个JavaScript的对象；
- redux-thunk可以让dispatch(action函数)，action**可以是一个函数**；
- 该函数会被调用，并且会传给这个函数一个dispatch函数和getState函数；
  - dispatch函数用于我们之后再次派发action；
  - getState函数考虑到我们之后的一些操作需要依赖原来的状态，用于让我们可以获取之前的一些状态；

## 十一、redux-thunk

详情：https://github.com/reduxjs/redux-thunk

在创建store时传入应用了middleware的enhance函数
- 通过applyMiddleware来结合多个Middleware, 返回一个enhancer；
- 将enhancer作为第二个参数传入到createStore中；

定义返回一个函数的action：
- 注意：这里不是返回一个对象了，而是一个函数；
- 该函数在dispatch之后会被执行；


[![Go to CodeSandbox](https://camo.githubusercontent.com/90808661433696bc57dce8d4ad732307b5cec6270e6b846f114dcd7ee7f9458a/68747470733a2f2f636f646573616e64626f782e696f2f7374617469632f696d672f706c61792d636f646573616e64626f782e737667)](https://codesandbox.io/embed/happy-babycat-7r5ri?fontsize=14&hidenavigation=1&theme=dark)

## 十二、redux-devtools

我们之前讲过，redux可以方便的让我们对状态进行跟踪和调试，那么如何做到呢？
- redux官网为我们提供了redux-devtools的工具；
- 利用这个工具，我们可以知道每次状态是如何被修改的，修改前后的状态变化等等；

安装该工具需要两步：
- 第一步：在对应的浏览器中安装相关的插件（比如Chrome浏览器扩展商店中搜索Redux DevTools即可，其他方法可以参考
GitHub）；
  - 下载地址：
    - https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd
    - https://chrome.zzzmh.cn/info?token=lmhkpmbekcpmknklioeibfkpmmfibljd
- 第二步：在redux中继承devtools的中间件；
  - https://github.com/zalmoxisus/redux-devtools-extension#12-advanced-store-setup


```jsx
import { createStore, applyMiddleware, compose } from "redux";
import thunk from "redux-thunk";
import reduer from "./reduer.jsx";

// 应用中间件
const enhancer = applyMiddleware(thunk);

const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;

const store = createStore(reduer, composeEnhancers(enhancer));

export default store;
```

![](https://gitee.com/itsandy/picgo-img/raw/master/react/redux-devtools.png)

## 十三、redux-saga

redux-saga是另一个比较常用在redux发送异步请求的中间件，它的使用更加的灵活。

Redux-saga的使用步骤如下
- 安装
- 集成redux-saga中间件
- saga.js文件的编写

详情：https://redux-saga.js.org/

[![Go to CodeSandbox](https://camo.githubusercontent.com/90808661433696bc57dce8d4ad732307b5cec6270e6b846f114dcd7ee7f9458a/68747470733a2f2f636f646573616e64626f782e696f2f7374617469632f696d672f706c61792d636f646573616e64626f782e737667)](https://codesandbox.io/embed/dazzling-shannon-s0o9x?fontsize=14&hidenavigation=1&theme=dark)

你会发现使用redux-saga会有些麻烦，而且是有点难的

其实使用react-thunk就能解决**日常的需求**，redux-saga在**大型项目**中用的比较多，而且你会发现redux-saga更加灵活，代码耦合度更低（把一些网络请求的代码都放在了saga文件中，而不是在action里显得很乱）

## 十四、redux文件拆分

### 14.1 reducer代码拆分

我们先来理解一下，为什么这个函数叫reducer？

这个其实在官网中就有说到https://redux.js.org/tutorials/essentials/part-1-overview-concepts#terminology

我们来看一下目前我们的reducer：
- 当前这个reducer既有处理counter的代码，又有处理home页面的数据；
- 后续counter相关的状态或home相关的状态会进一步变得更加复杂；
- 我们也会继续添加其他的相关状态，比如购物车、分类、歌单等等；
- 如果将所有的状态都放到一个reducer中进行管理，随着项目的日趋庞大，必然会造成代码臃肿、难以维护。

因此，我们可以对reducer进行拆分：
- 我们先抽取一个对counter处理的reducer；
- 再抽取一个对home处理的reducer；
- 将它们合并起来；

其实我们不需要自己来拆分，官方提供了一个合并reducer的函数叫做combineReducers

```jsx
import { ADD_NUMBER, SUB_NUMBER,RECOMMEND } from "./action-type.jsx";

const counterInitialState = {
  count: 0,
};

export const counterReduer = (state = counterInitialState, action) => {
  switch (action.type) {
    case ADD_NUMBER:
      return { ...state, count: state.count + action.num };
    case SUB_NUMBER:
      return { ...state, count: state.count - action.num };
    default:
      return state;
  }
};

const homeInitialState = {
  recommends: [],
};

export const homeReduer = (state = homeInitialState, action) => {
  switch (action.type) {
    case RECOMMEND:
      return { ...state, recommends: action.recommends };
    default:
      return state;
  }
};

const reducer = combineReducers({
  home,
  counter,
});

export default reducer;
```
### 14.2 redux文件拆分

目前我们已经将不同的状态处理拆分到不同的reducer中，我们来思考：
- 虽然已经放到不同的函数了，但是这些函数的处理依然是在同一个文件中，代码非常的混乱；
- 另外关于reducer中用到的action、action-type等我们也依然是在同一个文件中；

![](https://gitee.com/itsandy/picgo-img/raw/master/react/redux文件划分.png)


## 十五、Immutable 

我们先来看看前面说的reduer代码

```jsx
import { RECOMMEND } from "./action-type.jsx";

const homeInitialState = {
  recommends: [],
};

export const homeReduer = (state = homeInitialState, action) => {
  switch (action.type) {
    case RECOMMEND:
      return { ...state, recommends: action.recommends };
    default:
      return state;
  }
};
```

就来看看这段代码，你会发现我们每次返回的state都是将原来的initialState拷贝了一份，然后修改里面的部分属性

这里的initialState的数据很少，但是如果我们的initialState数据非常多，30行，100行，我们需要使用的state只有3，4条

但是它返回的state是全部的initialState（也就造成了一定的性能浪费）

当然你可能会想，我这样写不就好了吗(如果我只想要initialState里的一部分数据)

```jsx
import { RECOMMEND } from "./action-type.jsx";

const homeInitialState = {
  recommends: [],
};

export const homeReduer = (state = homeInitialState, action) => {
  switch (action.type) {
    case RECOMMEND:
      state.recommends = action.recommends;
      return state;
    default:
      return state;
  }
};
```

这样是不行的，我们前面说过redux要求我们reducr必须是一个**纯函数**

那我们怎么解决喃？

### 15.1 数据可变性的问题

在React开发中，我们总是会强调数据的不可变性：
- 无论是类组件中的state，还是redux中管理的state；
- 事实上在整个JavaScript编码过程中，数据的不可变性都是非常重要的；

数据的可变性引发的问题（案例）：

```js
const info = {
  name: 'tao',
  age: 18
}
const obj = info

info.name = 'sandy'
console.log(obj.name);  // sandy
```

我们明明没有修改obj，只是修改了info，但是最终obj也被我们修改掉了；

原因非常简单，对象是引用类型，它们指向同一块内存空间，两个引用都可以任意修改；

有没有办法解决上面的问题呢？
- 进行对象的拷贝即可：Object.assign或扩展运算符

这种对象的浅拷贝有没有问题呢？
- 从代码的角度来说，没有问题，也解决了我们实际开发中一些潜在风险；
- 从性能的角度来说，有问题，如果对象过于庞大，这种拷贝的方式会带来性能问题以及内存浪费；

有人会说，开发中不都是这样做的吗？
- 从来如此，便是对的吗？

### 15.2 认识ImmutableJS

为了解决上面的问题，出现了Immutable对象的概念：
- Immutable对象的特点是只要修改了对象，就会返回一个
新的对象，旧的对象不会发生改变；


但是这样的方式就不会浪费内存了吗？
- 为了节约内存，又出现了一个新的算法：Persistent Data
Structure（持久化数据结构或一致性数据结构）；

当然，我们一听到持久化第一反应应该是数据被保存到本地或
者数据库，但是这里并不是这个含义：
- 用一种数据结构来保存数据；
- 当数据被修改时，会返回一个对象，但是新的对象会尽可
能的利用之前的数据结构而不会对内存造成浪费；

![](https://gitee.com/itsandy/picgo-img/raw/master/react/ImmutableJS.gif)

### 15.3 ImmutableJS的基本使用

注意：我这里只是演示了一些API，更多的方式可以参考官网https://immutable-js.com/

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8" />
  <link rel="icon" type="image/svg+xml" href="/src/favicon.svg" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Vite App</title>
</head>

<body>
  <script src="https://cdn.jsdelivr.net/npm/immutable@4.0.0/dist/immutable.min.js"></script>
  <script>
    const im = Immutable
    const obj = {
      name: 'tao',
      age: 18,
      friend: {
        name: 'sandy',
        age: 21
      }
    }

    // Map(浅层转换)
    const objIM = im.Map(obj)
    const info = objIM
    const newObjIM = objIM.set('name', 'zm')
    console.log(info.get('name')) // tao
    console.log(newObjIM.get('name')) // zm

    // fromJS(深层转换)
    const obj2IM = im.fromJS(obj)
    const info2 = obj2IM
    const newObj2IM = obj2IM.set('friend', { name: 'ymy', age: 20 })
    console.log(info2.get('friend'))  
    console.log(newObj2IM.get('friend'))  //  {name: 'ymy', age: 20}

    // List
    const names = ['zs', 'ls', 'ww', 'zl']
    const namesIM = im.List(names)
    const arr = namesIM
    const newNamesIM = namesIM.set(0, 'tao')
    console.log(arr.get(0)) // zs
    console.log(newNamesIM.get(0))  // tao

  </script>

</body>

</html>
```

!>你会发现上面的Map方法很像ES6中的Map[详情跳转](javascript/es-next/es6?id=十三、map)

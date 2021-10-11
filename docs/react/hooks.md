# React Hooks

## 一、了解 Hook

### 1.1 为什么需要 Hook?

Hook 是 React 16.8 的新增特性，它可以让我们在不编写 class 的情况下使用 state 以及其他的 React 特性（比如生命周期）。

详情：https://zh-hans.reactjs.org/docs/hooks-intro.html

我们先来思考一下 class 组件相对于函数式组件有什么优势？比较常见的是下面的优势：

class 组件可以定义自己的 state，用来保存组件自己内部的状态；

- 函数式组件不可以，因为函数每次调用都会产生新的临时变量；

class 组件有自己的生命周期，我们可以在对应的生命周期中完成自己的逻辑；

- 比如在 componentDidMount 中发送网络请求，并且该生命周期函数只会执行一次；
- 函数式组件在学习 hooks 之前，如果在函数中发送网络请求，意味着每次重新渲染都会重新发送一次网络请求；

class 组件可以在状态改变时只会重新执行 render 函数以及我们希望重新调用的生命周期函数 componentDidUpdate 等；

- 函数式组件在重新渲染时，整个函数都会被执行，似乎没有什么地方可以只让它们调用一次；

所以，在 Hook 出现之前，对于上面这些情况我们通常都会编写 class 组件。

### 1.2 Class 组件存在的问题

复杂组件变得难以理解：

- 我们在最初编写一个 class 组件时，往往逻辑比较简单，并不会非常复杂。但是随着业务的增多，我们的 class 组件会变得越来越复杂；
- 比如 componentDidMount 中，可能就会包含大量的逻辑代码：包括网络请求、一些事件的监听（还需要在
  componentWillUnmount 中移除）；
- 而对于这样的 class 实际上非常难以拆分：因为它们的逻辑往往混在一起，强行拆分反而会造成过度设计，增加代码的复杂度；

难以理解的 class：

- 很多人发现学习 ES6 的 class 是学习 React 的一个障碍。
- 比如在 class 中，我们必须搞清楚 this 的指向到底是谁，所以需要花很多的精力去学习 this；
- 虽然我认为前端开发人员必须掌握 this，但是依然处理起来非常麻烦；

组件复用状态很难：

- 在前面为了一些状态的复用我们需要通过高阶组件或 render props；
- 比如 redux 中 connect 或者 react-router 中的 withRouter，这些高阶组件设计的目的就是为了状态的复用；
- 或者类似于 Provider、Consumer 来共享一些状态，但是多次使用 Consumer 时，我们的代码就会存在很多嵌套；
- 这些代码让我们不管是编写和设计上来说，都变得非常困难；

### 1.3 Hook 的出现

Hook 的出现，可以解决上面提到的这些问题；

简单总结一下 hooks：

- 它可以让我们在不编写 class 的情况下使用 state 以及其他的 React 特性；
- 但是我们可以由此延伸出非常多的用法，来让我们前面所提到的问题得到解决；

Hook 的使用场景：

- Hook 的出现基本可以代替我们之前所有使用 class 组件的地方（除了一些非常不常用的场景）；
- 但是如果是一个旧的项目，你并不需要直接将所有的代码重构为 Hooks，因为它完全向下兼容，你可以渐进式的来使用它；
- Hook 只能在函数组件中使用，不能在类组件，或者函数组件之外的地方使用；

### 1.4 计数器案例对比

我们通过一个计数器案例，来对比一下 class 组件和函数式组件结合 hooks 的对比：

```jsx
import React, { PureComponent } from "react";

export default class App extends PureComponent {
  constructor(props) {
    super(props);
    this.state = {
      count: 0,
    };
  }
  render() {
    const { count } = this.state;
    return (
      <div>
        <h2>当前计数：{count}</h2>
        <button onClick={() => this.setState({ count: count + 1 })}>+1</button>
        <button onClick={() => this.setState({ count: count - 1 })}>-1</button>
      </div>
    );
  }
}
```

```jsx
import React, { useState } from "react";

export default function App() {
  const [count, setCount] = useState(0);
  return (
    <div>
      <h2>当前计数：{count}</h2>
      <button onClick={() => setCount(count + 1)}>+1</button>
      <button onClick={() => setCount(count - 1)}>+1</button>
    </div>
  );
}
```

你会发现上面的代码差异非常大：函数式组件结
合 hooks 让整个代码变得非常简洁，并且再也不
用考虑 this 相关的问题；

## 二、Hooks

### 2.1 useState

那么我们来研究一下上面核心的一段代码代表什么意思：

useState 来自 react，需要从 react 中导入，它是一个 hook；

- 参数：初始化值，如果不设置为 undefined；
- 返回值：数组，包含两个元素；
  - 元素一：当前状态的值（第一调用为初始化值）；
  - 元素二：设置状态值的函数；
- 点击 button 按钮后，会完成两件事情：
  - 调用 setCount，设置一个新的值；
  - 组件重新渲染，并且根据新的值返回 DOM 结构；

**相信通过上面的一个简单案例，你已经会喜欢上 Hook 的使用了。**

- Hook 就是 JavaScript 函数，这个函数可以帮助你 钩入（hook into） React State 以及生命周期等特性；

但是使用它们会有两个额外的规则：

- 只能在`函数最外层`调用 Hook。不要在循环、条件判断或者子函数中调用。
- 只能在 `React 的函数组件`中调用 Hook。不要在其他 JavaScript 函数中调用。

State Hook 的 API 就是 useState，我们在前面已经进行了学习：

- useState 会帮助我们定义一个 state 变量，useState 是一种新方法，它与 class 里面的 this.state 提供的功能完全相同。一
  般来说，在函数退出后变量就会”消失”，而 state 中的变量会被 React 保留。
- useState 接受唯一一个参数，在第一次组件被调用时使用来作为初始化值。（如果没有传递参数，那么初始化值为
  undefined）。
- useState 是一个数组，我们可以通过数组的解构，来完成赋值会非常方便。

FAQ：为什么叫 useState 而不叫 createState?

- Create” 可能不是很准确，因为 state 只在组件首次渲染的时候被创建。
- 在下一次重新渲染时，useState 返回给我们当前的 state。
- 如果每次都创建新的变量，它就不是 “state”了。
- 这也是 Hook 的名字总是以 use 开头的一个原因。

当然，我们也可以在一个组件中定义多个变量和复杂变量（数组、对象）

```jsx
import React, { useState } from "react";

export default function App() {
  const [students, setStudents] = useState([
    { name: "tao", age: 18 },
    { name: "sandy", age: 21 },
    { name: "zm", age: 20 },
  ]);
  function changeAge(index) {
    const newStudents = [...students];
    newStudents[index].age += 1;
    setStudents(newStudents);
  }
  return (
    <div>
      <h2>学生列表</h2>
      <ul>
        {students.map((item, index) => {
          return (
            <li key={index}>
              {item.name}-{item.age}
              <button onClick={() => changeAge(index)}>年龄+1</button>
            </li>
          );
        })}
      </ul>
      <button
        onClick={() => setStudents([...students, { name: "ymy", age: 20 }])}
      >
        添加好友
      </button>
    </div>
  );
}
```

![](https://gitee.com/itsandy/picgo-img/raw/master/react/useState.gif)

useState 的默认值也可以传入一个函数，如果传入一个函数，就我们就可以作用上一次的 state 作为参数来对它进行操作（跟类组件的 setState 传入一个函数是一样的效果）

```jsx
import React, { useState } from "react";

export default function App() {
  const [count, setCount] = useState(() => 0);
  function increaseCount() {
    setCount(count + 10);
    setCount(count + 10);
    setCount(count + 10);
  }
  return (
    <div>
      <h2>当前计数：{count}</h2>
      <button onClick={() => setCount(count + 1)}>+1</button>
      {/* 点击了按钮，count = count +30 */}
      <button onClick={increaseCount}>+1</button>
    </div>
  );
}
```

### 2.2 useEffect

目前我们已经通过 hook 在函数式组件中定义 state，那么类似于生命周期这些呢？

- Effect Hook 可以让你来完成一些类似于 class 中生命周期的功能；
- 事实上，类似于网络请求、手动更新 DOM、一些事件的监听，都是 React 更新 DOM 的一些副作用（Side Effects）；
- 所以对于完成这些功能的 Hook 被称之为 Effect Hook；

假如我们现在有一个需求：页面的 title 总是显示 counter 的数字，分别使用 class 组件和 Hook 实现：

```jsx
import React, { PureComponent } from "react";

export default class App extends PureComponent {
  constructor(proprs) {
    super(proprs);
    this.state = {
      count: 0,
    };
  }
  componentDidMount() {
    document.title = this.state.count;
  }
  componentDidUpdate() {
    document.title = this.state.count;
  }
  render() {
    const { count } = this.state;
    return (
      <div>
        <h2>当前计数：{count}</h2>
        <button onClick={() => this.setState({ count: count + 1 })}>+1</button>
        <button onClick={() => this.setState({ count: count - 1 })}>-1</button>
      </div>
    );
  }
}
```

```jsx
import React, { useState, useEffect } from "react";

export default function App() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = count;
  });

  return (
    <div>
      <h2>当前技术：{count}</h2>
      <button onClick={() => setCount(count + 1)}>+1</button>
      <button onClick={() => setCount(count - 1)}>-1</button>
    </div>
  );
}
```

useEffect 的解析：

- 通过 useEffect 的 Hook，可以告诉 React 需要在渲染后执行某些操作；
- useEffect 要求我们传入一个回调函数，在 React 执行完更新 DOM 操作之后，就会回调这个函数；
- 默认情况下，无论是第一次渲染之后，还是每次更新之后，都会执行这个 回调函数；

在 class 组件的编写过程中，某些副作用的代码，我们需要在 componentWillUnmount 中进行清除：

- 比如我们之前的事件总线或 Redux 中手动调用 subscribe；
- 都需要在 componentWillUnmount 有对应的取消订阅；
- Effect Hook 通过什么方式来模拟 componentWillUnmount 呢？

useEffect 传入的回调函数 A 本身可以有一个返回值，这个返回值是另外一个回调函数 B：

```jsx
import React, { useState, useEffect } from "react";

function Home() {
  const [count, setCount] = useState(0);
  useEffect(() => {
    console.log("订阅了一些事件");
    return () => {
      console.log("取消了一些事件");
    };
  });
  return (
    <div>
      <h2>当前计数：{count}</h2>
      <button onClick={() => setCount(count + 1)}>+1</button>
      <button onClick={() => setCount(count - 1)}>-1</button>
    </div>
  );
}

export default function App() {
  const [isShow, setIsShow] = useState(true);
  return (
    <div>
      <button onClick={() => setIsShow(!isShow)}>显示/隐藏</button>
      {isShow && <Home />}
    </div>
  );
}
```

当我们点击显示/隐藏按钮的时候会打印'取消了一些事件'（没有问题），但是点击按钮的+1 或者-1 的时候，会打印‘取消了一些事件’,'订阅了一些事件'（这是为什么？）

还是那一张图

![](https://gitee.com/itsandy/picgo-img/raw/master/react/声明式编程.png)

不管是类组件还是函数式组件，都是 state 发生改变，就会重新执行函数（类中执行的是 render 函数，函数组件则会重新执行），函数执行完毕后 UI 就会重新渲染

所以点击+1 或者-1 的时候改变了 count 的值，所以整个 Home 组件会重新渲染，重新渲染就会重新执行里面的 useEffect 函数，重新渲染的时候会先执行 return 的函数，然后再执行里面的函数，所以先打印的是'取消了一些事件'，然后打印的是'订阅了一些事件`

!>useEffect Hook 参数的函数相对于是 componentDidMount,返回的函数相对于 componentWillUnmount

但是当我们组件更新时就取消了事件的话，这肯定是不符合我们的预期的，那怎么才能解决喃？

默认情况下，useEffect 的回调函数会在每次渲染时都重新执行，但是这会导致两个问题：

- 某些代码我们只是希望执行一次即可，类似于 componentDidMount 和 componentWillUnmount 中完成的事情；（比如网
  络请求、订阅和取消订阅）；
- 另外，多次执行也会导致一定的性能问题；

我们如何决定 useEffect 在什么时候应该执行和什么时候不应该执行呢？

- useEffect 实际上有两个参数：
- 参数一：执行的回调函数；
- 参数二：该 useEffect 在哪些 state 发生变化时，才重新执行；（受谁的影响）

那么为了解决上面的问题，我们就可以传入一个空的数组 []（表示这个 useEffect 函数只在第一次渲染和组件卸载的时候会执行一次，不管以后你的 DOM 怎么发生变化，这个函数都不会再执行）（**谁也不依赖**）

如果我们传入一个[count]，意思就是函数除了在第一次渲染和组件卸载的时候会执行一次，当 count 发生变化的时候，函数才会重新执行（其它的 DOM 发生变化都不会再执行）（**依赖 count**）

```jsx
import React, { useState, useEffect } from "react";

function Home() {
  const [count, setCount] = useState(0);
  useEffect(() => {
    console.log("订阅了一些事件");
    return () => {
      console.log("取消了一些事件");
    };
  }, []);
  return (
    <div>
      <h2>当前计数：{count}</h2>
      <button onClick={() => setCount(count + 1)}>+1</button>
      <button onClick={() => setCount(count - 1)}>-1</button>
    </div>
  );
}

export default function App() {
  const [isShow, setIsShow] = useState(true);
  return (
    <div>
      <button onClick={() => setIsShow(!isShow)}>显示/隐藏</button>
      {isShow && <Home />}
    </div>
  );
}
```

而且使用 Hook 的其中一个目的就是解决 class 中生命周期经常将很多的逻辑放在一起的问题：

- 比如网络请求、事件监听、手动修改 DOM，这些往往都会放在 componentDidMount 中；

使用 Effect Hook，我们可以将它们分离到不同的 useEffect 中：

```jsx
import React, { useEffect } from "react";

export default function App() {
  useEffect(() => {
    console.log("修改DOM");
  });
  useEffect(() => {
    console.log("订阅事件");
  });
  useEffect(() => {
    console.log("网络请求");
  });
  return (
    <div>
      <h2>Hello World!</h2>
    </div>
  );
}
```

Hook 允许我们按照代码的用途分离它们， 而不是像生命周期函数那样：

- React 将按照 effect 声明的顺序依次调用组件中的每一个 effect；

### 2.3 useContext

在之前的开发中，我们要在组件中使用共享的 Context 有两种方式：

- 类组件可以通过 类名.contextType = MyContext 方式，在类中获取 context；
- 多个 Context 或者在函数式组件中通过 MyContext.Consumer 方式共享 context；

但是多个 Context 共享时的方式会存在大量的嵌套：

- Context Hook 允许我们通过 Hook 来直接获取某个 Context 的值；

```jsx
import React, { useContext, createContext } from "react";

function Home() {
  const info = useContext(InfoContext);
  const theme = useContext(ThemeContext);
  return (
    <div>
      <h2 style={{ color: theme.color }}>info.name</h2>
      <h2 style={{ fontSize: theme.fontSize }}>info.age</h2>
    </div>
  );
}

const InfoContext = createContext();
const ThemeContext = createContext();
export default function App() {
  return (
    <div>
      <InfoContext.Provider value={{ name: "tao", age: 18 }}>
        <ThemeContext.Provider value={{ color: "red", fontSize: "30px" }}>
          <Home />
        </ThemeContext.Provider>
      </InfoContext.Provider>
    </div>
  );
}
```

### 2.4 useReducer

很多人看到 useReducer 的第一反应应该是 redux 的某个替代品，其实并不是。

useReducer 仅仅是 useState 的一种替代方案：

- 在某些场景下，如果 state 的处理逻辑比较复杂，我们可以通过 useReducer 来对其进行拆分；
- 或者这次修改的 state 需要依赖之前的 state 时，也可以使用；

```jsx
import React, { useReducer } from "react";

function reducer(state, action) {
  switch (action.type) {
    case "increment":
      return { ...state, count: state.count + 1 };
    case "decrement":
      return { ...state, count: state.count - 1 };
    default:
      return state;
  }
}

export default function App() {
  const [state, dispatch] = useReducer(reducer, { count: 0 });
  return (
    <div>
      <h2>当前计数：{state.count}</h2>
      <button onClick={() => dispatch({ type: "increment" })}>+1</button>
      <button onClick={() => dispatch({ type: "decrement" })}>-1</button>
    </div>
  );
}
```

数据是不会共享的，它们只是使用了相同的 counterReducer 的函数而已。

```jsx
import React, { useReducer } from "react";

function reducer(state, action) {
  switch (action.type) {
    case "increment":
      return { ...state, count: state.count + 1 };
    case "decrement":
      return { ...state, count: state.count - 1 };
    default:
      return state;
  }
}

function Home() {
  const [state, dispatch] = useReducer(reducer, { count: 0 });
  return (
    <div>
      <h2>当前计数：{state.count}</h2>
      <button onClick={() => dispatch({ type: "increment" })}>+1</button>
      <button onClick={() => dispatch({ type: "decrement" })}>-1</button>
    </div>
  );
}

export default function App() {
  const [state, dispatch] = useReducer(reducer, { count: 0 });
  return (
    <div>
      <h2>当前计数：{state.count}</h2>
      <button onClick={() => dispatch({ type: "increment" })}>+1</button>
      <button onClick={() => dispatch({ type: "decrement" })}>-1</button>
      <Home />
    </div>
  );
}
```

### 2.5 useCallback

useCallback 实际的目的是为了进行性能的优化

如何进行性能的优化呢？

- useCallback 会返回一个函数的 memoized（记忆的） 值；
- 在依赖不变的情况下，多次定义的时候，返回的值是相同的；

我们先来看一个案例

```jsx
import React, { useState, useCallback } from "react";

export default function App() {
  const [count, setCount] = useState(0);
  const [isShow, setIsShow] = useState(true);

  const increment = () => {
    console.log("increment重新渲染");
    setCount(count + 1);
  };
  const decrement = useCallback(() => {
    console.log("decrement重新渲染");
    setCount(count - 1);
  }, [count]);

  return (
    <div>
      <h2>当前计数：{count}</h2>
      <button onClick={increment}>+1</button>
      <button onClick={decrement}>-1</button>
      <button onClick={() => setIsShow(!isShow)}>切换</button>
    </div>
  );
}
```

我们简单来解析一下，当我们点击了+1/-1，count 发生了改变函数重新执行，意味着 incremenet 和 decrement 函数都会重新执行(这肯定是没问题的)

但是当我们点击了切换按钮的时候，isShow 发生了改变函数重新执行（你肯定会想 decrement 用了 useCallback 依赖了 count，这个函数就不会重新执行）

但其实不是这样的，useCallback 不管你依赖的值是否有没有发生改变，state 发生变化的时候这个 useCallback 函数都会重新执行，依赖的值只是决定了它返回的函数是否是同一个函数

比如我们点击的是-1 的按钮，，count 发生了改变，useCallback 重新执行，useCallback 依赖的 count，那么这次 useCallback 返回的函数和更新后的 useCallback 返回的函数是同一个函数，点击的是切换按钮，isShow 发生了改变，useCallback 重新执行，useCallback 依赖的 count，那么这次 useCallback 返回的函数和更新后的 useCallback 返回的函数就不是同一个函数

这个地方说起来可能有点绕，我画一个图应该就好理解一点

![](https://gitee.com/itsandy/picgo-img/raw/master/react/useCallback不能进行的性能优化.png)

!>所以不管 DOM 发生什么改变，useCallback 都会执行，上面的案例其实是没有带来性能上的优化的

那 useCallback 的使用场景是什么？

- 在将一个组件中的函数，传递给子元素进行回调使用时，使用 useCallback 对函数进行处理（可以达到一定程度上的性能优化）

```jsx
import React, { useState, useCallback } from "react";

function HomeIncremental(props) {
  console.log("HomeIncremental");
  return (
    <div>
      <button onClick={props.increment}>+1</button>
    </div>
  );
}
function HomeDecremental(props) {
  console.log("HomeDecremental");
  return (
    <div>
      <button onClick={props.decrement}>-1</button>
    </div>
  );
}

export default function App() {
  const [count, setCount] = useState(0);
  const [isShow, setIsShow] = useState(true);

  const increment = () => {
    console.log("increment重新渲染");
    setCount(count + 1);
  };
  const decrement = useCallback(() => {
    console.log("decrement重新渲染");
    setCount(count - 1);
  }, [count]);

  return (
    <div>
      <h2>当前计数：{count}</h2>
      <HomeIncremental increment={increment} />
      <HomeDecremental decrement={decrement} />
      <button onClick={() => setIsShow(!isShow)}>切换</button>
    </div>
  );
}
```

当我们点击子组件的+1/-1 的时候，其实触发的是父组件的 increment 和 decrement,修改的也是父组件的 state，state 发生了改变了，函数重新渲染，那么子组件也会重新渲染，意味着 HomeIncremental 和 HomeDecremental 组件都会重新渲染

为了处理不必要的渲染，我们以前会使用 memo

```jsx
import React, { useState, useCallback, memo } from "react";

const HomeIncremental = memo((props) => {
  console.log("HomeIncremental");
  return (
    <div>
      <button onClick={props.increment}>+1</button>
    </div>
  );
});

const HomeDecremental = memo((props) => {
  console.log("HomeDecremental");
  return (
    <div>
      <button onClick={props.decrement}>-1</button>
    </div>
  );
});

export default function App() {
  const [count, setCount] = useState(0);
  const [isShow, setIsShow] = useState(true);

  const increment = () => {
    console.log("increment重新渲染");
    setCount(count + 1);
  };
  const decrement = useCallback(() => {
    console.log("decrement重新渲染");
    setCount(count - 1);
  }, [count]);

  return (
    <div>
      <h2>当前计数：{count}</h2>
      <HomeIncremental increment={increment} />
      <HomeDecremental decrement={decrement} />
      <button onClick={() => setIsShow(!isShow)}>切换</button>
    </div>
  );
}
```

这个时候其实你会发现当我们点击+1/-1 的时候，HomeIncremental 和 HomeDecremental 组件都会重新渲染，当我们点击切换按钮的时候，HomeIncremental 组件会重新渲染，但是 HomeDecremental 组件不会重新渲染（这是为什么？，我们不是使用了 memo 了吗，为什么还会重新渲染）

我还是画一个图来说，比较方便一点

![](https://gitee.com/itsandy/picgo-img/raw/master/react/useCallback进行的性能优化.png)

### 2.6 useMemo

useMemo 实际的目的也是为了进行性能的优化。

如何进行性能的优化呢？

- useMemo 返回的也是一个 memoized（记忆的） 值；
- 在依赖不变的情况下，多次定义的时候，返回的值是相同的；

我们就来演示两个 useMemo 基本的使用场景

复杂计算的类型

```jsx
import React, { useState } from "react";

function addition(count) {
  console.log("重新计算");
  let result = 0;
  for (let i = 1; i < count; i++) {
    result += i;
  }
  return result;
}

// 计算1~count相加的结果
export default function App() {
  const [count, setCount] = useState(10);
  const [isShow, setIsShow] = useState(true);
  const result = addition(count);
  return (
    <div>
      <h2>计算结果：{result}</h2>
      <button onClick={() => setCount(count + 1)}>+1</button>
      <button onClick={() => setIsShow(!isShow)}>切换</button>
    </div>
  );
}
```

当我们点击+1 按钮就会重新计算 addition 相加的结果，但是当我们点击切换按钮还是会计算 addition 相加的结果（这显然是不行的）

state 发生改变，App 重新渲染，addition 重新执行，拿到新的 result 的值

为了解决这样的情况，我们就可以使用 useMemo

```jsx
import React, { useState, useMemo } from "react";

function addition(count) {
  console.log("重新计算");
  let result = 0;
  for (let i = 1; i < count; i++) {
    result += i;
  }
  return result;
}

// 计算1~count相加的结果
export default function App() {
  const [count, setCount] = useState(10);
  const [isShow, setIsShow] = useState(true);
  const result = useMemo(() => {
    return addition(count);
  }, [count]);

  return (
    <div>
      <h2>计算结果：{result}</h2>
      <button onClick={() => setCount(count + 1)}>+1</button>
      <button onClick={() => setIsShow(!isShow)}>切换</button>
    </div>
  );
}
```

传入子组件应用类型

```jsx
import React, { useState, memo } from "react";

const Home = memo((props) => {
  console.log("Home重新渲染");
  return (
    <div>
      <h2>{props.info.name}</h2>
      <h2>{props.info.age}</h2>
    </div>
  );
});

export default function App() {
  console.log("App重新渲染");
  const [isShow, setIsShow] = useState(true);

  const info = { name: "tao", age: 18 };
  return (
    <div>
      <Home info={info} />
      <button onClick={() => setIsShow(!isShow)}>切换</button>
    </div>
  );
}
```

我们点击切换按钮的时候，App 和 Home 都会重新渲染（App 重新渲染，会创建一个新的 info，传递给 Home 的就是一个新的 info,memo 就无效了，Home 就会重新渲染

有两种办法可以解决

- 把 info 设置为了 state
- 通过 useMemo 来进行优化

```jsx
import React, { useState, memo, useMemo } from "react";

const Home = memo((props) => {
  console.log("Home重新渲染");
  return (
    <div>
      <h2>{props.info.name}</h2>
      <h2>{props.info.age}</h2>
    </div>
  );
});

export default function App() {
  console.log("App重新渲染");
  const [isShow, setIsShow] = useState(true);

  const info = useMemo(() => {
    return { name: "tao", age: 18 };
  }, []);
  return (
    <div>
      <Home info={info} />
      <button onClick={() => setIsShow(!isShow)}>切换</button>
    </div>
  );
}
```

state 发生改变，App 重新渲染，useMemo 重新执行，但是 useMemo 谁也不依赖，所以返回的还是原来的对象，传递给 Home 的 info 还是原来的 info，Home 组件就不会重新渲染

你会发现 useCallback 和 useMemo 很像,useCallback 要求传入一个函数，在依赖的值没有发生改变的时候，返回的还是原来的**函数**，useMemo 要求传入一个回调函数，在依赖的值没有发生改变的时候，返回的**值**还是原来的值（useCallback 会自动返回回调函数，useMemo 需要手动返回一个值）

所以我们可以通过 useMemo 来实现 useCallback

```jsx
const decrement = useCallback(() => {
  console.log("decrement重新渲染");
  setCount(count - 1);
}, [count]);
const decrement = useMemo(() => {
  return () => {
    console.log("decrement重新渲染");
    setCount(count - 1);
  };
}, [count]);
```

### 2.7 useRef

useRef 返回一个 ref 对象，返回的 ref 对象再组件的整个生命周期保持不变。

最常用的 ref 是两种用法：

- 用法一：引入 DOM（或者组件，但是需要是 class 组件）元素；
- 用法二：保存一个数据，这个 ref 对象在整个生命周期中可以保存不变；

```jsx
import React, { useRef, forwardRef } from "react";

class Home extends React.PureComponent {
  render() {
    return <div>Home</div>;
  }
}

const About = forwardRef((props, ref) => {
  return <div ref={ref}>About</div>;
});

export default function App() {
  const titleRef = useRef();
  const inputRef = useRef();
  const homeRef = useRef();
  const aboutRef = useRef();

  function btnClick() {
    titleRef.current.innerHTML = "Hello React!";
    inputRef.current.focus();
    console.log(homeRef.current);
    aboutRef.current.innerHTML = "Hello About!";
  }

  return (
    <div>
      <h2 ref={titleRef}>Hello World!</h2>
      <input type="text" ref={inputRef} />
      <button onClick={btnClick}>按钮</button>
      <Home ref={homeRef} />
      <About ref={aboutRef} />
    </div>
  );
}
```

不仅可以获取元素或者组件，我们还可以保存一个数据，这个 ref 对象在整个生命周期中可以保持不变

```jsx
import React, { useRef } from "react";

export default function App() {
  const numRef = useRef(0);
  return (
    <div>
      <h2>当前计数：{numRef.current}</h2>
    </div>
  );
}
```

我们可以通过 useRef 的默认值来当做变量，useRef 在整个生命周期中返回的值都是相同的

那么这有什么用喃？

我们可以做一个记录当前的值和上一次值的案例

```jsx
import React, { useState, useRef, useEffect } from "react";

export default function App() {
  const [count, setCount] = useState(0);
  const numRef = useRef(count);
  useEffect(() => {
    numRef.current = count;
  }, [count]);
  return (
    <div>
      <h2>上一次的值：{numRef.current}</h2>
      <h2>这一次的值：{count}</h2>
      <button onClick={() => setCount(count + 1)}>+1</button>
    </div>
  );
}
```

有一个 count(state)，我们使用了 numRef 来接收 count 作为 useRef 的默认值，意味着 useRef 在整个生命周期中返回的值都是相同的（都是 count 默认的值 0）

但是下面有个 useEffect，当 count 发生改变，App 函数重新执行，jsx 重新渲染完之后就会执行 useEffect 函数（依赖 count），在 useEffect 中我们就可以让 numRef 的值为 count

意味着当我们第一次点击+1 的时候，App 函数重新执行，意味 numRef 永远记录的是 count 的值，所以 numRef.current 的值是 0，这一次的值是 1（count+1），jsx 渲染后，就会来到 useEffect 这个函数（依赖 count），让 numRef.current 的值等于 count（1），numRef.current 的值发生改变是不会触发更新的（因为 numRef.current 不是一个 state），所以 numRef.current 的值是 1,当我们第二次点击+1 的时候，App 函数重新执行，numRef.current 是 1，count+1（2），（所以上一次的值就是 1，这一次的值就是 2）jsx 渲染完毕，又会来到 useEffect 函数执行前面的操作，依次循环

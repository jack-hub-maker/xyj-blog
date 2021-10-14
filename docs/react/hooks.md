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

### 2.8 useImperativeHandle

我们先来回顾一下 ref 和 forwardRef 结合使用：

- 通过 forwardRef 可以将 ref 转发到子组件；
- 子组件拿到父组件中创建的 ref，可以对 ref 进行一些处理；

```jsx
import React, { forwardRef, useRef } from "react";

const Home = forwardRef((props, ref) => {
  return <input type="text" ref={ref} />;
});

export default function App() {
  const inputRef = useRef();
  function btnClick() {
    inputRef.current.focus();
  }
  return (
    <div>
      <Home ref={inputRef} />
      <button onClick={() => btnClick()}>按钮</button>
    </div>
  );
}
```

forwardRef 的做法本身没有什么问题，但是我们是将子组件的 DOM 直接暴露给了父组件：

- 直接暴露给父组件带来的问题是某些情况的不可控；
- 父组件可以拿到 DOM 后进行任意的操作；
- 但是，事实上在上面的案例中，我们只是希望父组件可以操作的 focus，并不希望它随意操作；

通过 useImperativeHandle 可以值暴露固定的操作：

- 通过 useImperativeHandle 的 Hook，将传入的 ref 和 useImperativeHandle 第二个参数返回的对象绑定到了一起；
- 所以在父组件中，使用 inputRef.current 时，实际上使用的是返回的对象；

```jsx
import React, { forwardRef, useRef, useImperativeHandle } from "react";

const Home = forwardRef((props, ref) => {
  const inputRef = useRef();
  useImperativeHandle(
    ref,
    () => ({
      focus() {
        inputRef.current.focus();
      },
    }),
    [inputRef.current]
  );
  return <input type="text" ref={inputRef} />;
});

export default function App() {
  const inputRef = useRef();
  function btnClick() {
    inputRef.current.focus();
  }
  return (
    <div>
      <Home ref={inputRef} />
      <button onClick={() => inputRef.current.focus()}>按钮</button>
    </div>
  );
}
```

我们把 inputRef 绑定到 Home 上，因为绑定的是一个函数式组件，我们需要通过 forwardRef 进行转发，ref 就会被转发到第二个参数，我们就可以通过 useImperativeHandle 对 ref 进行一些处理（也可以理解为拦截），返回了一个对象（对象里面有一个 focus 方法），意味着父组件绑定给我们的 inputRef 的 current 上有一个 focus 方法，
父组件的按钮点击触发的 inputRef.current.focus 方法其实是触发的是 useImperativeHandle 里返回的对象里的 focus 方法

接着子组件自身创建了一个属于自己的 inputRef，把它绑定到了自己的 input 上，所以父组件的 inputRef.current.focus 方法里的触发的 inputRef.current.focus,就是通过子组件自身的 inputRef 调用里面的 focus 方法（说起来有点绕，其实仔细看一下代码应该是能看懂的）

其实我们可以试验一下，input 标签都有一个 placeholder 属性的，如果我们不通过 useImperativeHandle 来对它进行处理的话，父组件是可以随便调用 input 里的东西的(例如 placeholder)

```jsx
import React, { useRef, forwardRef } from "react";

const Home = forwardRef((props, ref) => {
  return <input type="text" ref={ref} />;
});

export default function App() {
  const inputRef = useRef();
  function btnClick() {
    inputRef.current.focus();
    inputRef.current.placeholder = "Hello World";
  }
  return (
    <div>
      <Home ref={inputRef} />
      <button onClick={btnClick}>按钮</button>
    </div>
  );
}
```

![](https://gitee.com/itsandy/picgo-img/raw/master/react/没有useImperativeHandle.gif)

如果我们通过 useImperativeHandle 对转发后的 ref 进行处理（拦截），就可以实现父组件只能调用子组件提供给我们的东西，而不是可以任意操作子组件的东西

```jsx
import React, { useRef, forwardRef, useImperativeHandle } from "react";

const Home = forwardRef((props, ref) => {
  const inputRef = useRef();
  useImperativeHandle(
    ref,
    () => ({
      focus() {
        inputRef.current.focus();
      },
    }),
    [inputRef.current]
  );
  return <input type="text" ref={inputRef} />;
});

export default function App() {
  const inputRef = useRef();
  function btnClick() {
    inputRef.current.focus();
    inputRef.current.placeholder = "Hello World";
  }
  return (
    <div>
      <Home ref={inputRef} />
      <button onClick={btnClick}>按钮</button>
    </div>
  );
}
```

我们打印 inputRef.current 里面只有子组件提供的 focus 方法，所以操作 input 的 placeholder 是没有效果的

![](https://gitee.com/itsandy/picgo-img/raw/master/react/有useImperativeHandle.gif)

### 2.9 useLayoutEffect

useLayoutEffect 看起来和 useEffect 非常的相似，事实上他们也只有一点区别而已：

- useEffect 会在渲染的内容更新到 DOM 上后执行，不会阻塞 DOM 的更新；
- useLayoutEffect 会在渲染的内容更新到 DOM 上之前执行，**会阻塞**DOM 的更新；

如果我们希望在某些操作发生之后再更新 DOM，那么应该将这个操作放到 useLayoutEffect。

```jsx
import React, { useState, useEffect } from "react";

export default function App() {
  const [count, setCount] = useState(5);
  useEffect(() => {
    if (count === 0) {
      setCount(Math.random() + 200);
    }
  }, [count]);
  return (
    <div>
      <h2>数字：{count}</h2>
      <button onClick={() => setCount(0)}>修改数字</button>
    </div>
  );
}
```

点击修改数字按钮，组件重新渲染，先等 DOM 渲染完，再执行 useEffect

数字就会出现两次渲染，先点击修改数字，count 会从 5 变成 0，然后会执行 useEffect 函数，count= 0，会修改 state

state 发生改变，又会重新渲染 ，等 DOM 渲染完，再执行 useEffect，此时 count！=0，就没有任何操作了

而且我们看到屏幕从闪烁的效果，从 0 变成 200 多的随机数（当然这里看的不是很清楚，其实是有的）

![](https://gitee.com/itsandy/picgo-img/raw/master/react/使用useLayoutEffect前.gif)

这个时候我们就可以使用 useLayoutEffect 来进行优化

```jsx
import React, { useState, useLayoutEffect } from "react";

export default function App() {
  const [count, setCount] = useState(5);
  useLayoutEffect(() => {
    if (count === 0) {
      setCount(Math.random() + 200);
    }
  }, [count]);
  return (
    <div>
      <h2>数字：{count}</h2>
      <button onClick={() => setCount(0)}>修改数字</button>
    </div>
  );
}
```

点击修改数字按钮，组件重新渲染，DOM 渲染完执行 useEffect，然后再渲染 DOM

数字只会出现一次渲染，先点击修改数字，数字从 5 变成了 0（但是没有渲染出来），会在 DOM 渲染完前会执行 useLayoutEffect 函数，count= 0，会修改 state

state 发生改变，又会重新渲染 ，DOM 渲染前执行 useLayoutEffect 此时 count！=0，就没有任何操作了，然后再更新 DOM

这个时候你就会发现没有任何的闪烁效果了

![](https://gitee.com/itsandy/picgo-img/raw/master/react/使用useLayoutEffect后.gif)

!>useLayoutEffect 其实在大多数情况下都使用不到的，除了一些非常特殊的场景会使用到

### 2.10 useDebugValue

useDebugValue 可用于在 React 开发者工具中显示自定义 hook 的标签。

这个不是非常重要，可以自行查看

https://zh-hans.reactjs.org/docs/hooks-reference.html#usedebugvalue

## 三、自定义 Hook

### 3.1 了解自定义 Hook

自定义 Hook 本质上只是一种函数代码逻辑的抽取，严格意义上来说，它本身并不算 React 的特性。

需求：所有的组件在创建和销毁时都进行打印

- 组件被创建：打印 组件被创建了；
- 组件被销毁：打印 组件被销毁了；

```jsx
import React, { useState, useEffect } from "react";

function publicLifeCycle(name) {
  useEffect(() => {
    console.log(`${name}组件被创建了`);
    return () => {
      console.log(`${name}组件被销毁了`);
    };
  }, []);
}

function Home() {
  publicLifeCycle("home");
  return (
    <div>
      home
      <About />
      <Profile />
    </div>
  );
}

function About() {
  publicLifeCycle("about");
  return <div>About</div>;
}

function Profile() {
  publicLifeCycle("profile");
  return <div>Profile</div>;
}

export default function App() {
  const [isShow, setIsShow] = useState(true);
  return (
    <div>
      {isShow && <Home />}
      <button onClick={() => setIsShow(!isShow)}>切换</button>
    </div>
  );
}
```

虽然这样可以使用，这是因为我是在 jsx 文件中编写的，如果是在 js 文件中编写这样的代码是会报错的

React 官方也严格说过只能在 React 函数中使用 Hook，这是必须遵守的（这个时候我们就可以自定义 Hook）

```jsx
import React, { useState, useEffect } from "react";

function usePublicLifeCycle(name) {
  useEffect(() => {
    console.log(`${name}组件被创建了`);
    return () => {
      console.log(`${name}组件被销毁了`);
    };
  }, []);
}

function Home() {
  usePublicLifeCycle("home");
  return (
    <div>
      home
      <About />
      <Profile />
    </div>
  );
}

function About() {
  usePublicLifeCycle("about");
  return <div>About</div>;
}

function Profile() {
  usePublicLifeCycle("profile");
  return <div>Profile</div>;
}

export default function App() {
  const [isShow, setIsShow] = useState(true);
  return (
    <div>
      <button onClick={() => setIsShow(!isShow)}>切换</button>
      {isShow && <Home />}
    </div>
  );
}
```

你会发现跟上面的函数没什么区别，只是我们封装的函数是以**use**开头的

自定义 Hook 是一个函数，其名称以 “use” 开头，函数内部可以调用其他的 Hook。

换句话说，它就像一个正常的函数。但是它的名字应该始终以 use 开头，这样可以一眼看出其符合 Hook 的规则

接着我们就来做几个案例来练习一下

### 3.2 自定义 Hook 练习

#### 3.2.1 共享 Context

有了 useContext，我们可能会这样共享 Context

```jsx
import React, { createContext } from "react";
import Home from "./Home.jsx";

export const UserContext = createContext();
export const ThemeContext = createContext();

export default function App() {
  return (
    <div>
      <UserContext.Provider value={{ name: "tao", age: 18 }}>
        <ThemeContext.Provider value={{ color: "red", fontSize: "30px" }}>
          <Home />
        </ThemeContext.Provider>
      </UserContext.Provider>
    </div>
  );
}
```

```jsx
import React, { useContext } from "react";
import { ThemeContext, UserContext } from "./App";

function About() {
  const user = useContext(UserContext);
  const theme = useContext(ThemeContext);
  return (
    <div>
      <h2 style={{ color: theme.color }}>{user.name}</h2>
      <h2 style={{ color: theme.fontSize }}>{user.age}</h2>
    </div>
  );
}

export default function Home() {
  const user = useContext(UserContext);
  const theme = useContext(ThemeContext);
  return (
    <div>
      <h2 style={{ color: theme.color }}>{user.name}</h2>
      <h2 style={{ color: theme.fontSize }}>{user.age}</h2>
      <hr />
      <About />
    </div>
  );
}
```

这样做有什么弊端喃？

1.  第一我们需要知道我们的 Context 叫什么名字，然后通过 useContext 来使用
2.  当多个组件需要使用的时候，有时候我们需要创建多个 useContext，也会导入多个 Context

那么通过自定义 hook 就可以解决

```jsx
import { useContext } from "react";
import { UserContext, ThemeContext } from "../App";

export default function useTaoContext() {
  const user = useContext(UserContext);
  const theme = useContext(ThemeContext);
  return [user, theme];
}
```

接着我们直接使用里面的 Context 就可以了，对比一下代码你就会发现这样非常简单，如果是像以前通过高阶组件来进行 Context 来共享的话，整个代码会非常简洁，阅读性也会强很多

```jsx
import React from "react";
import useTaoContext from "./hooks/user-hook";

function About() {
  const [user, theme] = useTaoContext();
  return (
    <div>
      <h2 style={{ color: theme.color }}>{user.name}</h2>
      <h2 style={{ color: theme.fontSize }}>{user.age}</h2>
    </div>
  );
}

export default function Home() {
  const [user, theme] = useTaoContext();
  return (
    <div>
      <h2 style={{ color: theme.color }}>{user.name}</h2>
      <h2 style={{ color: theme.fontSize }}>{user.age}</h2>
      <hr />
      <About />
    </div>
  );
}
```

#### 3.2.2 获取鼠标滚动位置

获取鼠标滚动位置

```jsx
import React, { useState, useEffect } from "react";

export default function App() {
  const [scroll, setScroll] = useState(0);

  useEffect(() => {
    const handleScroll = () => {
      setScroll(window.scrollY);
    };
    document.addEventListener("scroll", handleScroll);
    return () => {
      document.removeEventListener("scroll", handleScroll);
    };
  }, []);
  return (
    <div style={{ padding: "1000px 0" }}>
      <h2 style={{ position: "fixed", left: "0", top: "0" }}>
        距离顶部:{scroll}
      </h2>
    </div>
  );
}
```

如果我们多个组件都想获取，就可以封装成 hook

```jsx
import { useState, useEffect } from "react";
export default function useScrollPositon() {
  const [scroll, setScroll] = useState(0);

  useEffect(() => {
    const handleScroll = () => {
      setScroll(window.scrollY);
    };
    document.addEventListener("scroll", handleScroll);
    return () => {
      document.removeEventListener("scroll", handleScroll);
    };
  }, []);
  return scroll;
}
```

```jsx
import React from "react";
import useScrollPositon from "./hooks/scroll-position-hook";
export default function App() {
  const scroll = useScrollPositon();
  return (
    <div style={{ padding: "1000px 0" }}>
      <h2 style={{ position: "fixed", left: "0", top: "0" }}>
        距离顶部:{scroll}
      </h2>
    </div>
  );
}
```

#### 3.2.3 localStorage数据存储

```jsx
import React, { useState, useEffect } from "react";

export default function App() {
  const [name, setName] = useState(() => {
    // 如果localStorage里有数据就直接拿出来
    const name = JSON.parse(localStorage.getItem("name"));
    return name;
  });
  useEffect(() => {
    window.localStorage.setItem("name", JSON.stringify(name));
  }, [name]);
  return (
    <div>
      <h2>当前存储:{name}</h2>
      <button onClick={() => setName("tao")}>设置name</button>
    </div>
  );
}

```

我们可以封装一个hook

```jsx
import { useState, useEffect } from "react";

export default function useLocalStoreage(name) {
  const [name, setName] = useState(() => {
    // 如果localStorage里有数据就直接拿出来
    const name = JSON.parse(localStorage.getItem("name"));
    return name;
  });
  useEffect(() => {
    window.localStorage.setItem("name", JSON.stringify(name));
  }, [name]);
  return [name, setName];
}

```

```jsx
import React from "react";
import useLocalStoreage from "./hooks/local-store-hook";

export default function App() {
  const [name, setName] = useLocalStoreage("name");
  return (
    <div>
      <h2>当前存储:{name}</h2>
      <button onClick={() => setName("tao")}>设置name</button>
    </div>
  );
}
```
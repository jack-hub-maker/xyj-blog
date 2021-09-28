## React 更新机制

react 的渲染机制：

![image.png](https://img14.360buyimg.com/ddimg/jfs/t1/204762/30/7600/29177/614b3617E5bc5eb33/39eb3315dc97a269.png)

react 的更新流程：

![image.png](https://img13.360buyimg.com/ddimg/jfs/t1/202913/4/7435/39373/614b371eE4c98a8b2/f51c479d6bb107a0.png)

## React 的更新流程

React 在 props 或 state 发生改变时，会调用 React 的 render 方法，会创建一颗不同的树。

React 需要基于这两颗不同的树之间的差别来判断如何有效的更新 UI：

- 如果一棵树参考另外一棵树进行完全比较更新，那么即使是最先进的算法，该算法的复杂程度为 O(n 3 )，其中 n 是树中元素
  的数量；
- 如果在 React 中使用了该算法，那么展示 1000 个元素所需要执行的计算量将在十亿的量级范围；
- 这个开销太过昂贵了，React 的更新性能会变得非常低效；

于是，React 对这个算法进行了优化，将其优化成了 O(n)，如何优化的呢？

- 同层节点之间相互比较，不会垮节点比较；
- 不同类型的节点，产生不同的树结构；
- 开发中，可以通过 key 来指定哪些节点在不同的渲染下保持稳定；

### 情况一：对比不同类型的元素

当节点为不同的元素，React 会拆卸原有的树，并且建立起新的树：

- 当一个元素从 a 变成 img，从 Article 变成 Comment，或从 Button 变成 div 都会触发一个完整的重建
  流程；
- 当卸载一棵树时，对应的 DOM 节点也会被销毁，组件实例将执行 componentWillUnmount() 方法；
- 当建立一棵新的树时，对应的 DOM 节点会被创建以及插入到 DOM 中，组件实例将执行 componentWillMount() 方法，
  紧接着 componentDidMount() 方法；

### 情况二：对比同一类型的元素

当比对两个相同类型的 React 元素时，React 会保留 DOM 节点，仅比对及更新有改变的属性。

比如下面的代码更改：

- 通过比对这两个元素，React 知道只需要修改 DOM 元素上的 className 属性；

```html
<h2 className="before"></h2>
<h2 className="after"></h2>
```

比如下面的代码更改：

- 当更新 style 属性时，React 仅更新有所更变的属性。
- 通过比对这两个元素，React 知道只需要修改 DOM 元素上的 color 样式，无需修改 fontWeight。

```html
<div style={{ color: "red", fontWeight: "bold" }}></div>
<div style={{ color: "green", fontWeight: "bold" }}></div>
```

如果是同类型的组件元素：

- 组件会保持不变，React 会更新该组件的 props，并且调用 componentWillReceiveProps() 和 componentWillUpdate() 方
  法；
- 下一步，调用 render() 方法，diff 算法将在之前的结果以及新的结果中进行递归；

### 情况三：对子节点进行递归

在默认条件下，当递归 DOM 节点的子元素时，React 会同
时遍历两个子元素的列表；当产生差异时，生成一个
mutation。

- 我们来看一下在最后插入一条数据的情况：
- 前面两个比较是完全相同的，所以不会产生 mutation；
- 最后一个比较，产生一个 mutation，将其插入到新的
  DOM 树中即可；

但是如果我们是在中间插入一条数据：

- React 会对每一个子元素产生一个 mutation，而不是保
  持 星际穿越和盗梦空间不变；
- 这种低效的比较方式会带来一定的性能问题；

```html
<ul>
  <li>first</li>
  <li>second</li>
</ul>

<ul>
  <li>first</li>
  <li>second</li>
  <li>footer</li>
</ul>
```

```html
<ul>
  <li>大话西游</li>
  <li>唐伯虎点秋香</li>
</ul>

<ul>
  <li>大话西游</li>
  <li>功夫</li>
  <li>唐伯虎点秋香</li>
</ul>
```

## keys 的优化

当你遍历列表的时候，总是会提示一个警告，让我们添加一个 key 属性：

方式一：在最后位置插入数据

- 这种情况，有无 key 意义并不大

方式二：在前面插入数据

- 这种做法，在没有 key 的情况下，所有的 li 都需要进行修改；
- 当子元素拥有 key 时，React 使用 key 来匹配原有树上的子元素以及最新树上的子元素：

key 的注意事项：

- key 应该是唯一的；
- key 不要使用随机数（随机数在下一次 render 时，会重新生成一个数字）；
- 使用 index 作为 key，对性能是没有优化的；

## render 函数被调用

还是哪张图

![](https://img11.360buyimg.com/ddimg/jfs/t1/78488/2/17033/218092/6142b204E568116f5/690af293487b7b88.png)

当依赖的数据(state、props)发生变化时，再调用自己的 render 方法

如何来控制 render 方法是否被调用呢？

通过 shouldComponentUpdate 方法即可；

## shouldComponentUpdate

详情：https://zh-hans.reactjs.org/docs/optimizing-performance.html#avoid-reconciliation

React 给我们提供了一个生命周期方法 shouldComponentUpdate（很多时候，我们简称为 SCU），这个方法接受参数，并且
需要有返回值：

该方法有两个参数：

- 参数一：nextProps 修改之后，最新的 props 属性
- 参数二：nextState 修改之后，最新的 state 属性

该方法返回值是一个 boolean 类型

- 返回值为 true，那么就需要调用 render 方法；
- 返回值为 false，那么久不需要调用 render 方法；
- 默认返回的是 true，也就是只要 state 发生改变，就会调用 render 方法；

比如我们在 App 中增加一个 message 属性：

- jsx 中并没有依赖这个 message，那么它的改变不应该引起重新渲染；
- 但是因为 render 监听到 state 的改变，就会重新 render，所以最后 render 方法还是被重新调用了；

```js
import React, { Component } from "react";

class Son extends Component {
  render() {
    console.log("Son函数重新渲染了");
    return <div>Son</div>;
  }
}

export default class App extends Component {
  constructor() {
    super();
    this.state = {
      count: 0,
      message: "Hello World",
    };
  }
  render() {
    console.log("App函数重新渲染了");
    return (
      <div>
        <h2>当前计数：{this.state.count}</h2>
        <h2>{this.state.message}</h2>
        <button onClick={(e) => this.btnClick()}>+1</button>
        <button onClick={(e) => this.changeMessage()}>改变文本</button>
        <Son />
      </div>
    );
  }
  shouldComponentUpdate(nextProps, nextState) {
    if (this.state.count !== nextState.count) {
      return true;
    }
    return false;
  }
  btnClick() {
    this.setState({
      count: this.state.count + 1,
    });
  }
  changeMessage() {
    this.setState({
      message: "Hello React",
    });
  }
}
```

## PureComponent

详情：https://zh-hans.reactjs.org/docs/render-props.html#caveats

如果所有的类，我们都需要手动来实现 shouldComponentUpdate，那么会给我们开发者增加非常多的工作量。

事实上 React 已经考虑到了这一点，所以 React 已经默认帮我们实现好了，如何实现呢？

- 将 class 继承自 PureComponent。

```js
class Son extends PureComponent {
  render() {
    console.log("Son函数重新渲染了");
    return <div>Son</div>;
  }
}
```

## memo

详情：https://zh-hans.reactjs.org/docs/react-api.html#reactmemo

上面介绍的都是类的使用，函数中提供了一个叫做 memo 的高阶组件也可以做到类似的效果

```js
const Son2 = memo(() => {
  console.log("Son2函数重新渲染了");
  return <div>Son2</div>;
});
```

所以以后我们创建类组件或者是函数组件都可以使用 PureComponent 和 memo

## 不可变数据的力量

详情：https://zh-hans.reactjs.org/docs/optimizing-performance.html#the-power-of-not-mutating-data

我们先来做一个案例就会理解不可变数据是什么意思了

我们展示了一个列表，然后点击按钮，向列表中添加一位

![](https://gitee.com/itsandy/picgo-img/raw/master/react/friends.png)

为什么在上面的代码中不建议直接对原来的数组进行 push 修改 state，而是添加一个新数组进行改变或者使用扩展运算符来进行

这其实很简单，如果你对 JavaScript 比较熟悉的话就可以清晰的知道为什么？

方便理解我画了个内存图来进行讲解

![](https://gitee.com/itsandy/picgo-img/raw/master/react/immutable.png)

对象，数组我们都知道是 JavaScript 中的引用类型，如果使用的话会在堆内存中开辟一块空间的（保存着内存地址）

我们使用 friedns 变量赋值为一个数组，其实就是创建了 friedns 保存了数组的内存地址，通过内存地址找到了数组（指向/引用数组）

数组中的对象也是这样的，看上去数组中一块一块的是对象，其实也是保存了对象的内存地址，通过内存地址找到了各自的对象

虽然我们进行了 push，但是还是通过内存地址在数组的空间里创建了一个对象

然后修改 friends：this.state.friends，其实上就是 friends：friends，这其实是同一个对象，所以并没有发生改变，没有发生改变，界面就不会进行渲染

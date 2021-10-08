# Fragments

在之前的开发中，我们总是在一个组件中返回内容时包裹一个 div 元素：

我们又希望可以不渲染这样一个 div 应该如何操作呢？使用 Fragment

详情：https://zh-hans.reactjs.org/docs/fragments.html

优化前

```js
import React, { PureComponent } from "react";

export default class App extends PureComponent {
  render() {
    return (
      <div>
        <h2>Hello World!</h2>
      </div>
    );
  }
}
```

![](https://gitee.com/itsandy/picgo-img/raw/master/react/fragments优化前.png)

优化后

```js
import React, { PureComponent, Fragment } from "react";

export default class App extends PureComponent {
  render() {
    return (
      <Fragment>
        <h2>Hello World!</h2>
      </Fragment>
    );
  }
}
```

![](https://gitee.com/itsandy/picgo-img/raw/master/react/fragment优化后.png)

还有一种语法糖<>,</>,但是使用语法糖的时候是不能往它上面添加key或者属性


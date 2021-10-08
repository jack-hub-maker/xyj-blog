# CSS-in-JS

## 一、组件化天下的 CSS

就目前来看，整个前端已经是组件化的天下：

- 而 CSS 的设计就不是为组件化而生的，所以在目前组件化的框架中都在需要一种合适的 CSS 解决方案。
  在组件化中选择合适的 CSS 解决方案应该符合以下条件：
- 可以编写局部 css：css 具备自己的具备作用域，不会随意污染其他组件内的原生；
- 可以编写动态的 css：可以获取当前组件的一些状态，根据状态的变化生成不同的 css 样式；
- 支持所有的 css 特性：伪类、动画、媒体查询等；
- 编写起来简洁方便、最好符合一贯的 css 风格特点；
- 等等...

## 二、React 中的 CSS

事实上，css 一直是 React 的痛点，也是被很多开发者吐槽、诟病的一个点。

在这一点上，Vue 做的要确实要好于 React：

- Vue 通过在.vue 文件中编写 style 标签来编写自己的样式；
- 通过是否添加 scoped 属性来决定编写的样式是全局有效还是局部有效；
- 通过 lang 属性来设置你喜欢的 less、sass 等预处理器；
- 通过内联样式风格的方式来根据最新状态设置和改变 css；
- 等等...

Vue 在 CSS 上虽然不能称之为完美，但是已经足够简洁、自然、方便了，至少统一的样式风格不会出现多个开发人员、多个项目
采用不一样的样式风格。

相比而言，React 官方并没有给出在 React 中统一的样式风格：

- 由此，从普通的 css，到 css modules，再到 css in js，有几十种不同的解决方案，上百个不同的库；
- 大家一致在寻找最好的或者说最适合自己的 CSS 方案，但是到目前为止也没有统一的方案；

## 三、写法

### 3.1 内联样式

内联样式是官方推荐的一种 css 样式的写法：

- style 接受一个采用小驼峰命名属性的 JavaScript 对象，，而不是 CSS 字符串；
- 并且可以引用 state 中的状态来设置相关的样式；

```jsx
import React, { PureComponent } from "react";

export default class App extends PureComponent {
  render() {
    const pStyle = {
      color: "green",
      fontSize: "50px",
    };
    return (
      <div>
        <h2 style={{ color: "red", fontSize: "50px" }}>Hello World!</h2>
        <p style={pStyle}>我是p元素</p>
      </div>
    );
  }
}
```

内联样式的优点:

- 内联样式, 样式之间不会有冲突
- 可以动态获取当前 state 中的状态

内联样式的缺点：

- 写法上都需要使用驼峰标识
- 某些样式没有提示
- 大量的样式, 代码混乱
- 某些样式无法编写(比如伪类/伪元素)

所以官方依然是希望内联合适和普通的 css 来结合编写；

### 3.2 普通的 css

普通的 css 我们通常会编写到一个单独的文件，之后再进行引入。

这样的编写方式和普通的网页开发中编写方式是一致的：

- 如果我们按照普通的网页标准去编写，那么也不会有太大的问题；
- 但是组件化开发中我们总是希望组件是一个独立的模块，即便是样式也只是在自己内部生效，不会相互影响；
- 但是普通的 css 都属于全局的 css，样式之间会相互影响；

这种编写方式最大的问题是样式之间会相互层叠掉；

### 3.3 css modules

css modules 并不是 React 特有的解决方案，而是所有使用了类似于 webpack 配置的环境下都可以使用的。

但是，如果在其他项目中使用个，那么我们需要自己来进行配置，比如配置 webpack.config.js 中的 modules: true 等。

详情：https://github.com/css-modules/css-modules

React 的脚手架已经内置了 css modules 的配置：

- .css/.less/.scss 等样式文件都修改成 .module.css/.module.less/.module.scss 等；
- 之后就可以引用并且进行使用了；

```css
.title {
  color: red;
  font-size: 30px;
}
.pStyle {
  color: green;
  font-size: 50px;
}
```

```jsx
import React, { PureComponent } from "react";
import style from "./style.module.css";

export default class App extends PureComponent {
  render() {
    return (
      <div>
        <h2 className={style.title}>Hello World!</h2>
        <p className={style.pStyle}>我是p元素</p>
      </div>
    );
  }
}
```

css modules 确实解决了局部作用域的问题，也是`很多人喜欢`在 React 中使用的一种方案。

但是这种方案也有自己的缺陷：

- 引用的类名，不能使用连接符(.home-title)，在 JavaScript 中是不识别的；
- 所有的 className 都必须使用{style.className} 的形式来编写；
- 不方便动态来修改某些样式，依然需要使用内联样式的方式；

如果你觉得上面的缺陷还算 OK，那么你在开发中完全可以选择使用 css modules 来编写，并且也是在 React 中很受欢迎的一种方
式。

### 3.4 CSS-in-JS

实际上，官方文档也有提到过 CSS in JS 这种方案：

详情：https://zh-hans.reactjs.org/docs/faq-styling.html

- “CSS-in-JS” 是指`一种模式`，其中 CSS 由 JavaScript 生成而不是在外部文件中定义；
- 注意此功能并不是 React 的一部分，而是由第三方库提供。 React 对样式如何定义并没有明确态度；

在传统的前端开发中，我们通常会将结构（HTML）、样式（CSS）、逻辑（JavaScript）进行分离。

- 但是在前面的学习中，我们就提到过，React 的思想中认为逻辑本身和 UI 是无法分离的，所以才会有了 JSX 的语法。
- 样式呢？样式也是属于 UI 的一部分；
- 事实上 CSS-in-JS 的模式就是一种将样式（CSS）也写入到 JavaScript 中的方式，并且可以方便的使用 JavaScript 的状态；
- 所以 React 有被人称之为 `All in JS`；

当然，这种开发的方式也受到了很多的批评：

- Stop using CSS in JavaScript for web development
- https://hackernoon.com/stop-using-css-in-javascript-for-web-development-fa32fb873dcc

!> 个人而言，你喜欢那种就用那种，团队开发用那种就用那种（你不喜欢就跑路）

## 四、styled-components

批评声音虽然有，但是在我们看来很多优秀的 CSS-in-JS 的库依然非常强大、方便：

- CSS-in-JS 通过 JavaScript 来为 CSS 赋予一些能力，包括类似于 CSS 预处理器一样的样式嵌套、函数定义、逻辑复用、动态修
  改状态等等；
- 依然 CSS 预处理器也具备某些能力，但是获取动态状态依然是一个不好处理的点；
- 所以，目前可以说 CSS-in-JS 是 React 编写 CSS 最为受欢迎的一种解决方案；

目前比较流行的 CSS-in-JS 的库有哪些呢？

- styled-components
- emotion
- glamorous

目前可以说 styled-components 依然是社区最流行的 CSS-in-JS 库，所以我们以 styled-components 的讲解为主；

详情：https://styled-components.com/

可以先看[第五章](#五、es6-标签模板字符串)再来看这里

### 4.1 styled 的基本使用

styled-components 的本质是通过函数的调
用，最终创建出一个组件：

- 这个组件会被自动添加上一个不重复的
  **class**；
- styled-components 会给该 class 添加相
  关的样式；

另外，它支持类似于 CSS 预处理器一样的样
式嵌套：

- 支持直接子代选择器或后代选择器，并且
  直接编写样式；
- 可以通过&符号获取当前元素；
- 直接伪类选择器、伪元素等；

```jsx
import React, { PureComponent } from "react";
import styled from "styled-components";

const TitleContainer = styled.h2`
  color: blue;
  font-size: 30px;
`;

const PContainer = styled.p`
  color: red;
  font-size: 30px;
`;

export default class App extends PureComponent {
  render() {
    return (
      <div>
        <TitleContainer>Hello World!</TitleContainer>
        <PContainer>我是一个p标签</PContainer>
      </div>
    );
  }
}
```

### 4.2 styled 的高级特性

如果我们想控制元素的属性，我们可以给他添加 attrs 属性

详情：https://styled-components.com/docs/faqs#when-to-use-attrs

```jsx
import React, { PureComponent } from "react";
import styled from "styled-components";

const TAOInput = styled.input.attrs((props) => ({
  type: props.TypeAttr,
}))``;

export default class App extends PureComponent {
  constructor(props) {
    super(props);
    this.state = {
      inputType: "password",
    };
  }
  render() {
    return (
      <div>
        <TAOInput type="text" TypeAttr={this.state.inputType} />
        {/*
         * 解释一下这里的用法
            给input添加了TypeAttr属性，属性值为password
            attrs可以传一个函数，参数props，这里的props可以拿到组件传给我们的props
            看上去是给input传了个TypeAttr属性，其实这里的input已经是TAOInput的组件实例了
            给组件实例上添加属性（不就是相对于给组件传递props嘛）
            所以上面就可以拿到传递过来的props，然后就可以使用props
            因为attrs可以控制组件实例的属性，这里就设置了input的type属性为props.TypeAttr = password;
            (我的个人理解吧，但感觉一些词语用的不是很恰当)
         */}
      </div>
    );
  }
}
```

props **可以穿透**

props 可以被传递给 styled 组件

- 获取 props 需要通过${}传入一个插值函数，props 会作为该函数的参数；
- 这种方式可以有效的解决动态样式的问题；

```jsx
import React, { PureComponent } from "react";
import styled from "styled-components";

const TAOInput = styled.input.attrs((props) => ({
  type: props.TypeAttr,
  bColor: props.color,
}))`
  border-color: red;
  background-color: ${(props) => props.bColor};
`;
/* props既可以使用组件实例传递过来的props，又可以使用attrs中的属性（穿透性） */
/* 为什么调用了attrs函数后面还可以通过``调用函数喃，这是因为attrs函数返回了一个新的函数，之后我们可以通过``调用新的函数（柯里化） */

export default class App extends PureComponent {
  constructor(props) {
    super(props);
    this.state = {
      inputType: "password",
      color: "green",
    };
  }
  render() {
    return (
      <div>
        <TAOInput
          type="text"
          TypeAttr={this.state.inputType}
          color={this.state.color}
        />
      </div>
    );
  }
}
```

styled 还支持样式的继承

```jsx
import React, { PureComponent } from "react";
import styled from "styled-components";

const TAOButton = styled.button`
  color: red;
  font-size: 30px;
  padding: 20px, 10px;
`;

// 这样就继承了TAOButton中的所有样式
const TAOButton2 = styled(TAOButton)`
  background-color: green;
`;

export default class App extends PureComponent {
  constructor(props) {
    super(props);
    this.state = {
      inputType: "password",
      color: "green",
    };
  }
  render() {
    return (
      <div>
        <TAOButton>按钮</TAOButton>
        <TAOButton2>按钮</TAOButton2>
      </div>
    );
  }
}
```

styled 也可以设置主题

```jsx
import React, { PureComponent } from "react";
import styled, { ThemeProvider } from "styled-components";

const TAOButton = styled.button`
  color: ${(props) => props.theme.themeColor};
  font-size: 30px;
  padding: 20px, 10px;
`;
// 它的内部封装了Context，我们直接使用就可以了
export default class App extends PureComponent {
  render() {
    return (
      <ThemeProvider theme={{ themeColor: "red" }}>
        <TAOButton>按钮</TAOButton>
      </ThemeProvider>
    );
  }
}
```

!>唯一不方便的是这个代码提示太差了

## 五、ES6 标签模板字符串

ES6 中增加了模板字符串的语法，这个对于很多人来说都会使用。

但是模板字符串还有另外一种用法：标签模板字符串（Tagged Template
Literals）。

我们一起来看一个普通的 JavaScript 的函数：

正常情况下，我们都是通过 函数名() 方式来进行调用的，其实函数还有另外
一种调用方式：

如果我们在调用的时候插入其他的变量：

- 模板字符串被拆分了；
- 第一个元素是数组，是被模块字符串拆分的字符串组合；
- 后面的元素是一个个模块字符串传入的内容；

```js
function foo(...args) {
  console.log(args);
}
const name = "tao";
const age = 18;
foo`my name is ${name} and i am ${age} years old`;
```

![](https://gitee.com/itsandy/picgo-img/raw/master/react/标签模板字符串.png)

在 styled component 中，就是通过这种方式来解析模块字符串，最终生成
我们想要的样式的

## 六、classnames

React 在 JSX 给了我们开发者足够多的灵活性，你可以像编写 JavaScript 代码一样，通过一些逻辑来决定是否添加某些 class：

```jsx
import React, { PureComponent } from "react";

export default class App extends PureComponent {
  constructor(props) {
    super(props);
    this.state = {
      isShow: true,
    };
  }
  render() {
    return (
      <div>
        <h2 className={"foo bar baz"}></h2>
        <h2 className={"foo " + (this.state.isShow ? "bar" : "")}></h2>
        <h2 className={["foo ", this.state.isShow ? "bar" : ""].join("")}></h2>
      </div>
    );
  }
}
```

![](https://gitee.com/itsandy/picgo-img/raw/master/react/classname.png)

但其实这样在开发中可能不是很方便

这个时候我们可以借助于一个第三方的库：classnames

详情：https://github.com/JedWatson/classnames

```jsx
import React, { PureComponent } from "react";
import classNames from "classnames";

export default class App extends PureComponent {
  constructor(props) {
    super(props);
    this.state = {
      isShow: true,
    };
  }
  render() {
    return (
      <div>
        <h2 className={classNames("foo", "bar", "baz")}></h2>
        <h2 className={classNames("foo", { bar: this.state.isShow })}></h2>
        <h2 className={classNames("foo", { bar: this.state.isShow })}></h2>
      </div>
    );
  }
}
```

![](https://gitee.com/itsandy/picgo-img/raw/master/react/classnames.png)

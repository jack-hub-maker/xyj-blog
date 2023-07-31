# React Router

## 一、了解react-router
目前前端流行的三大框架, 都有自己的路由实现:
- Angular的ngRouter
- React的react-router
- Vue的vue-router
  
React Router的版本4开始，路由不再集中在一个包中进行管理了：
- react-router是router的核心部分代码；
- react-router-dom是用于浏览器的；
- react-router-native是用于原生应用的；

目前我们使用最新的React Router版本是v5的版本：
- 实际上v4的版本和v5的版本差异并不大；

详情：https://reactrouter.com/web/guides/quick-start

## 二、Router的基本使用

react-router最主要的API是给我们提供的一些组件：

BrowserRouter或HashRouter
- Router中包含了对路径改变的监听，并且会将相应的路径传递给子组件；
- BrowserRouter使用history模式；
- HashRouter使用hash模式；

Link和NavLink：
- 通常路径的跳转是使用Link组件，最终会被渲染成a元素；
- NavLink是在Link基础之上增加了一些样式属性（后续学习）；
- to属性：Link中最重要的属性，用于设置跳转到的路径；

Route：
- Route用于路径的匹配；
- path属性：用于设置匹配到的路径；
- component属性：设置匹配到路径后，渲染的组件；
- exact：精准匹配，只有精准匹配到完全一致的路径，才会渲染对应的组件；

```jsx
import React from "react";
import { BrowserRouter as Router, Switch, Route, Link } from "react-router-dom";

function Home() {
  return <h2>Home</h2>;
}

function About() {
  return <h2>About</h2>;
}

function Users() {
  return <h2>Users</h2>;
}

export default function App() {
  return (
    <Router>
      <Link to="/">home</Link>
      <Link to="/about">about</Link>
      <Link to="/users">users</Link>
      <Route exact path="/" component={Home} />
      <Route path="/about" component={About} />
      <Route path="/users" component={Users} />
    </Router>
  );
}
```

## 三、NavLink的使用


需求：路径选中时，对应的a元素变为红色

这个时候，我们要使用NavLink组件来替代Link组件：

- activeStyle：活跃时（匹配时）的样式；
- activeClassName：活跃时添加的class；
- exact：是否精准匹配；

```jsx
import React from "react";
import {
  BrowserRouter as Router,
  Switch,
  Route,
  NavLink,
} from "react-router-dom";
import "./app.css";

function Home() {
  return <h2>Home</h2>;
}

function About() {
  return <h2>About</h2>;
}

function Users() {
  return <h2>Users</h2>;
}

export default function App() {
  return (
    <Router>
      <NavLink exact to="/" activeClassName="link-active">
        home
      </NavLink>
      <NavLink to="/about" activeClassName="link-active">
        about
      </NavLink>
      <NavLink to="/users" activeClassName="link-active">
        users
      </NavLink>
      <Route exact path="/" component={Home} />
      <Route path="/about" component={About} />
      <Route path="/users" component={Users} />
    </Router>
  );
}

```

```css
a.link-active {
  color: red;
}
```

## 四、Switch的作用

前面的代码你会发现是存在bug的，比如/about路径匹配到的同时，最后的一个NoMatch组件总是被匹配到；

原因是什么呢？默认情况下，react-router中只要是路径被匹配到的Route对应的组件都会被渲染；
- 但是实际开发中，我们往往希望有一种排他的思想：
- 只要匹配到了第一个，那么后面的就不应该继续匹配了；
- 这个时候我们可以使用Switch来将所有的Route进行包裹即可；

```jsx
import React from "react";
import {
  BrowserRouter as Router,
  Switch,
  Route,
  NavLink,
} from "react-router-dom";
import "./app.css";

function Home() {
  return <h2>Home</h2>;
}

function About() {
  return <h2>About</h2>;
}

function Users() {
  return <h2>Users</h2>;
}
function NoPath() {
  return <h2>NoPath</h2>;
}

export default function App() {
  return (
    <Router>
      <NavLink exact to="/" activeClassName="link-active">
        home
      </NavLink>
      <NavLink to="/about" activeClassName="link-active">
        about
      </NavLink>
      <NavLink to="/users" activeClassName="link-active">
        users
      </NavLink>
      <Route exact path="/" component={Home} />
      <Route path="/about" component={About} />
      <Route path="/users" component={Users} />
      <Route component={NoPath} />
    </Router>
  );
}

```

## 五、Redirect的作用

Redirect用于路由的重定向，当这个组件出现时，就会执行跳转到对应的to路径中：

我们这里使用这个的一个案例：
- 用户跳转到User界面；
- 但是在User界面有一个isLogin用于记录用户是否登录：
  - true：那么显示用户的名称；
  - false：直接重定向到登录界面

这里的useState，不需要理解，先用着（后面讲解Hooks的时候会详细解答）
```jsx
import React, { useState } from "react";
import {
  BrowserRouter as Router,
  Switch,
  Route,
  NavLink,
  Redirect,
} from "react-router-dom";
import "./app.css";

function Home() {
  return (
    <div>
      <h2>Home</h2>
    </div>
  );
}

function Users() {
  const [isLogin, setIsLogin] = useState(false);
  return isLogin ? <div>用户名：tao</div> : <Redirect to="login" />;
}
function Login() {
  return <h2>Login</h2>;
}
function NoPath() {
  return <h2>NoPath</h2>;
}

export default function App() {
  return (
    <Router>
      <NavLink exact to="/" activeClassName="link-active">
        home
      </NavLink>
      <NavLink exact to="/about" activeClassName="link-active">
        about
      </NavLink>
      <NavLink exact to="/users" activeClassName="link-active">
        users
      </NavLink>
      <NavLink exact to="/login" activeClassName="link-active">
      login
      </NavLink>
      <Switch>
        <Route exact path="/" component={Home} />
        <Route path="/users" component={Users} />
        <Route path="/login" component={Login} />
        <Route component={NoPath} />
      </Switch>
    </Router>
  );
}
```

## 六、路由的嵌套

在开发中，路由之间是存在嵌套关系的。

这里我们假设about页面中有两个页面内容：
- 商品列表和消息列表；
- 点击不同的链接可以跳转到不同的地方，显示不同的内容；

```jsx
import React from "react";
import {
  BrowserRouter as Router,
  Switch,
  Route,
  NavLink,
} from "react-router-dom";
import "./app.css";

function Home() {
  return <h2>Home</h2>;
}

function About() {
  return (
    <div>
      <Router>
        <NavLink exact to="/about/goods" activeClassName="link-active">
          goods
        </NavLink>
        <NavLink to="/about/message" activeClassName="link-active">
          message
        </NavLink>
        <Switch>
          <Route exact path="/about/goods" component={Goods} />
          <Route path="/about/message" component={Message} />
          <Route component={NoPath} />
        </Switch>
      </Router>
    </div>
  );
}
function Goods() {
  return <h2>Goods</h2>;
}
function Message() {
  return <h2>Message</h2>;
}
function NoPath() {
  return <h2>NoPath</h2>;
}

export default function App() {
  return (
    <Router>
      <NavLink exact to="/" activeClassName="link-active">
        home
      </NavLink>
      <NavLink to="/about/goods" activeClassName="link-active">
        about
      </NavLink>
      <Switch>
        <Route exact path="/" component={Home} />
        <Route path="/about/goods" component={About} />
        <Route path="/about/message" component={Message} />
        <Route component={NoPath} />
      </Switch>
    </Router>
  );
}
```

## 七、手动路由跳转


目前我们实现的跳转主要是通过Link或者NavLink进行跳转的，实际上我们也可以通过JavaScript代码进行跳转。

但是通过JavaScript代码进行跳转有一个前提：必须获取到history对象。

如何可以获取到history的对象呢？两种方式
1.  如果该组件是**通过路由直接跳转**过来的，那么可以直接获取history、location、match对象；
2.  如果该组件是一个普通渲染的组件，那么不可以直接获取history、location、match对象；

那么如果普通的组件也希望获取对应的对象属性应该怎么做呢？
- 前面我们学习过高阶组件，可以在组件中添加想要的属性；
- react-router也是通过高阶组件为我们的组件添加相关的属性的；

如果我们希望在App组件中获取到history对象，必须满足一下两个条件：
- App组件必须包裹在Router组件之内；
- App组件使用withRouter高阶组件包裹；

```jsx
import React from "react";
import {
  BrowserRouter as Router,
  Switch,
  Route,
  NavLink,
  withRouter,
} from "react-router-dom";
import "./app.css";

function Home() {
  return <h2>Home</h2>;
}

function About(props) {
  return (
    <div>
      <Router>
        <NavLink exact to="/about" activeClassName="link-active">
          Goods
        </NavLink>
        <NavLink to="/about/message" activeClassName="link-active">
          message
        </NavLink>
        <button onClick={() => props.history.push("/about/me")}>Me</button>
        <Switch>
          <Route exact path="/about" component={Goods} />
          <Route path="/about/message" component={Message} />
          <Route path="/about/me" component={Me} />
          <Route component={NoPath} />
        </Switch>
      </Router>
    </div>
  );
}
function Goods() {
  return <h2>Goods</h2>;
}
function Message() {
  return <h2>Message</h2>;
}
function Me() {
  return <h2>Me</h2>;
}
function User() {
  return <h2>User</h2>;
}
function NoPath() {
  return <h2>NoPath</h2>;
}

function App(props) {
  return (
    <Router>
      <NavLink exact to="/" activeClassName="link-active">
        home
      </NavLink>
      <NavLink to="/about" activeClassName="link-active">
        about
      </NavLink>
      <button onClick={() => props.history.push("/user")}>User</button>
      <Switch>
        <Route exact path="/" component={Home} />
        <Route path="/about" component={About} />
        <Route path="/user" component={User} />
        <Route component={NoPath} />
      </Switch>
    </Router>
  );
}
export default withRouter(App);
```
```jsx
import React from "react";
import ReactDOM from "react-dom";
import { BrowserRouter as Router } from "react-router-dom";
import App from "./App";

ReactDOM.render(
  <Router>
    <App />
  </Router>,
  document.getElementById("root")
);

```


看图我们会发现，url改变了，但是页面没有刷新

这我觉得应该是个bug，后来通过网上解决的，给组件添加一个key属性

```jsx
import React from "react";
import {
  BrowserRouter as Router,
  Switch,
  Route,
  NavLink,
  withRouter,
} from "react-router-dom";
import "./app.css";

function Home() {
  return <h2>Home</h2>;
}

function About(props) {
  return (
    <div>
      <Router key={props.location.key}>
        <NavLink exact to="/about" activeClassName="link-active">
          Goods
        </NavLink>
        <NavLink to="/about/message" activeClassName="link-active">
          message
        </NavLink>
        <button onClick={() => props.history.push("/about/me")}>Me</button>
        <Switch>
          <Route exact path="/about" component={Goods} />
          <Route path="/about/message" component={Message} />
          <Route path="/about/me" component={Me} />
          <Route component={NoPath} />
        </Switch>
      </Router>
    </div>
  );
}
function Goods() {
  return <h2>Goods</h2>;
}
function Message() {
  return <h2>Message</h2>;
}
function Me() {
  return <h2>Me</h2>;
}
function User() {
  return <h2>User</h2>;
}
function NoPath() {
  return <h2>NoPath</h2>;
}

function App(props) {
  return (
    <Router key={props.location.key}>
      <NavLink exact to="/" activeClassName="link-active">
        home
      </NavLink>
      <NavLink to="/about" activeClassName="link-active">
        about
      </NavLink>
      <button onClick={() => props.history.push("/user")}>User</button>
      <Switch>
        <Route exact path="/" component={Home} />
        <Route path="/about" component={About} />
        <Route path="/user" component={User} />
        <Route component={NoPath} />
      </Switch>
    </Router>
  );
}
export default withRouter(App);

```

这样就解决了



## 八、参数传递

传递参数有三种方式：
1.  动态路由的方式；
2.  search传递参数(不推荐)；
3.  Link中to传入对象；

动态路由的概念指的是路由中的路径并不会固定：
- 比如/detail的path对应一个组件Detail；
- 如果我们将path在Route匹配时写成/detail/:id，那么 /detail/abc、/detail/123都可以匹配到该Route，并且进行显示；
- 这个匹配规则，我们就称之为动态路由；

```jsx
import React from "react";
import {
  BrowserRouter as Router,
  Switch,
  Route,
  NavLink,
} from "react-router-dom";
import "./app.css";

function Home() {
  return <h2>Home</h2>;
}
function Details(props) {
  // console.log(props.match);
  const id = props.match.params.id;
  return <h2>传递过来的参数:{id}</h2>;
}
function NoPath() {
  return <h2>NoPath</h2>;
}

export default function App() {
  const id = "aaa";
  return (
    <Router>
      <NavLink exact to="/" activeClassName="link-active">
        home
      </NavLink>
      <NavLink to={`/details/${id}`} activeClassName="link-active">
        details
      </NavLink>
      <Switch>
        <Route exact path="/" component={Home} />
        <Route path="/details/:id" component={Details} />
        <Route component={NoPath} />
      </Switch>
    </Router>
  );
}

```

search传递参数

```jsx
import React from "react";
import {
  BrowserRouter as Router,
  Switch,
  Route,
  NavLink,
} from "react-router-dom";
import "./app.css";

function Home() {
  return <h2>Home</h2>;
}
function Details(props) {
  console.log(props.location);
  // serarch参数转为对象
  const id = props.location.search;
  const searchParams = new URLSearchParams(props.location.search);
  const searchObj = Object.fromEntries(searchParams);
  console.log(searchObj);
  return (
    <h2>
      传递过来的参数:{searchObj.name},{searchObj.age}
    </h2>
  );
}
function NoPath() {
  return <h2>NoPath</h2>;
}

export default function App() {
  const id = "aaa";
  return (
    <Router>
      <NavLink exact to="/" activeClassName="link-active">
        home
      </NavLink>
      <NavLink to={`/details?name=tao&age=18`} activeClassName="link-active">
        details
      </NavLink>
      <Switch>
        <Route exact path="/" component={Home} />
        <Route path="/details" component={Details} />
        <Route component={NoPath} />
      </Switch>
    </Router>
  );
}

```

Link中to可以直接传入一个对象(官方比较推荐这种方法)

```jsx
import React from "react";
import {
  BrowserRouter as Router,
  Switch,
  Route,
  NavLink,
} from "react-router-dom";
import "./app.css";

function Home() {
  return <h2>Home</h2>;
}
function Details(props) {
  // console.log(props.location);
  const info = props.location.state;
  return <h2>传递过来的参数:{info.height}</h2>;
}
function NoPath() {
  return <h2>NoPath</h2>;
}

export default function App() {
  return (
    <Router>
      <NavLink exact to="/" activeClassName="link-active">
        home
      </NavLink>
      <NavLink
        to={{
          pathname: "/details",
          search: "?name=tao&age=18",
          state: { height: 1.88 },
        }}
        activeClassName="link-active"
      >
        details
      </NavLink>
      <Switch>
        <Route exact path="/" component={Home} />
        <Route path="/details" component={Details} />
        <Route component={NoPath} />
      </Switch>
    </Router>
  );
}

```

## 九、react-router-config

目前我们所有的路由定义都是直接使用Route组件，并且添加属性来完成的。

但是这样的方式会让路由变得非常混乱，我们希望将所有的路由配置放到一个地方进行集中管理：

这个时候可以使用react-router-config来完成；

详情：https://www.npmjs.com/package/react-router-config

配置路由映射的关系数组

使用renderRoutes函数完成配置

```jsx
import Home from "../Home";
import About from "../About";
import Message from "../Message";
import Me from "../Me";

const routes = [
  {
    path: "/",
    exact: true,
    component: Home,
  },
  {
    path: "/about",
    exact: true,
    component: About,
    routes: [
      {
        path: "/about",
        exact: true,
        component: Message,
      },
      {
        path: "/about/me",
        exact: true,
        component: Me,
      },
    ],
  },
];
export default routes;

```

```jsx
import React from "react";
import { BrowserRouter as Router, NavLink } from "react-router-dom";
import { renderRoutes } from "react-router-config";
import routes from "./router";
import "./app.css";

export default function App() {
  return (
    <Router>
      <NavLink exact to="/" activeClassName="link-active">
        home
      </NavLink>
      <NavLink exact to="/about" activeClassName="link-active">
        about
      </NavLink>
      {renderRoutes(routes)}
    </Router>
  );
}

```

```jsx
import React from "react";
import { BrowserRouter as Router, NavLink } from "react-router-dom";
import { renderRoutes } from "react-router-config";
import "./app.css";

export default function About(props) {
  console.log(props.route);
  return (
    <div>
      <Router>
        <NavLink exact to="/about" activeClassName="link-active">
          message
        </NavLink>
        <NavLink to="/about/me" activeClassName="link-active">
          me
        </NavLink>
        {renderRoutes(props.route.routes)}
      </Router>
    </div>
  );
}

```
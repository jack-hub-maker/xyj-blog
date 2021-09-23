## 认识 JSX

详情：

- https://zh-hans.reactjs.org/docs/introducing-jsx.html
- https://zh-hans.reactjs.org/docs/jsx-in-depth.html

JSX 是什么？

- JSX 是一种 JavaScript 的语法扩展（eXtension），也在很多地方称之为 JavaScript XML，因为看起就是一段 XML 语法；
- 它用于描述我们的 UI 界面，并且其完成可以和 JavaScript 融合在一起使用；
- 它不同于 Vue 中的模块语法，你不需要专门学习模块语法中的一些指令（比如 v-for、v-if、v-else、v-bind）；

## 为什么 React 选择了 JSX

以前我们学习的时候，不是都说 html，css，js 代码要分离吗？

所有的东西耦合在一起是不好的吗？

但是 react 不是这样的，react 有一种哲学叫做`all in js`

前面也说到了声明式编程，我觉得那张图非常适合 react

![](https://img11.360buyimg.com/ddimg/jfs/t1/78488/2/17033/218092/6142b204E568116f5/690af293487b7b88.png)

JSX 的书写规范：

- JSX 的顶层只能有一个根元素（跟 vue2 的 template 一样），所以我们很多时候会在外层包裹一个 div 原生（或者使用后面我们学习的 Fragment）；
- 为了方便阅读，我们通常在 jsx 的外层包裹一个小括号()，这样可以方便阅读，并且 jsx 可以进行换行书写；

## JSX 的使用

jsx 中的注释

```js
<div>
  {/*我是注释 */}
  Hello World
</div>
```

对象不能作为 jsx 的子类（不能直接展示对象）

```js
<div>{this.state.info}</div>
```

![image.png](https://img13.360buyimg.com/ddimg/jfs/t1/203536/14/7128/7284/61455341Ef05d0f7c/0e949b80ca93c41f.png)

对象作为 React 子对象无效,如果您想呈现一组子元素，请使用数组

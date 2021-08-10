## Vite 脚手架工具

- 在开发中，我们不可能所有的项目都使用 vite 从零去搭建，比如一个 react 项目、Vue 项目；
  - 这个时候 vite 还给我们提供了对应的脚手架工具；
- 所以 Vite 实际上是有两个工具的：
  - vite：相当于是一个构件工具，类似于 webpack、rollup；
  - @vitejs/create-app：类似 vue-cli、create-react-app；
- 如果使用脚手架工具呢？

```sh
npm init @vitejs/app <project-name>
```

- 上面的做法相当于省略了安装脚手架的过程：

```sh
npm install @vitejs/create-app -g create-app
```

![12.png](https://img11.360buyimg.com/ddimg/jfs/t1/178292/33/18532/45265/61122c4eE44152a5c/7a4ab4b21616c9ff.png)
![13.png](https://img12.360buyimg.com/ddimg/jfs/t1/193245/39/17759/87443/61122c4eEaaab326a/46b724d8830e59c2.png)
![14.png](https://img11.360buyimg.com/ddimg/jfs/t1/182334/32/18487/48025/61122c4fE7a25fa3c/03648a1429ce213c.png)

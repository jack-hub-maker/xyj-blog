首先介绍两个网站

1.  [Axios 官网](https://axios-http.com/)
2.  [测试网站](http://httpbin.org/)

接下来我们就来简单的来演示使用 axios 发送网络请求

这里我用的是一个接口来进行演示
```js
axios({
  url: "http://httpbin.org/get",
}).then((res) => {
  console.log(res);
});
```

通过 axios 的实例调用,可以传入一个 url,实例本身返回一个 Promise,成功请求到数据之后会在内部把状态修改为 reslove,之后就可以调用 then 方法,拿到请求过来的数据

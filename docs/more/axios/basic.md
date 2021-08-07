首先介绍两个网站

1.  [Axios 官网](https://axios-http.com/zh/)
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

通过 axios 的实例调用,可以传入一个 url,实例本身返回一个 **Promise**,成功请求到数据之后会在内部把状态修改为 reslove,之后就可以调用 then 方法,拿到请求过来的数据

如果你想指定请求的类型的话,可以在 method 写上对应的请求方式 GET/POST(**默认**是 GET 请求)

```js
axios({
  url: "http://httpbin.org/post",
  method: "POST",
}).then((res) => {
  console.log(res);
});
```

我们不仅可以使用上面这种形式发送网络请求,axios 还为我们提供了很多方法,比如 axios.get,axios.post 等

```js
axios.get("http://httpbin.org/get", {}).then((res) => {
  console.log(res);
});
axios.post("http://httpbin.org/post", {}).then((res) => {
  console.log(res);
});
```

如果 url 的后面想跟参数的话也是可以的,而且我们不会直接在后面添加参数,而是把参数写在 params 里

```js
axios({
  url: "http://httpbin.org/get",
  params: {
    name: "gt",
    age: 20,
  },
}).then((res) => {
  console.log(res);
});
```

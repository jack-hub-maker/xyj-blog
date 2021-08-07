如果你想同时发送多个请求的话,可以使用 axios.all

```js
axios
  .all([
    axios.get("http://httpbin.org/get"),
    axios.post("http://httpbin.org/post"),
  ])
  .then(
    axios.spread((res1, res2) => {
      console.log(res1);
      console.log(res2);
    })
  );
```

这个方法会等待多个请求**都完成**之后,才会执行对应的 then 或者 catch 方法

axios.all 返回的结果是一个数组,使用 axios.spread 可以将数组展开来(类似于数组的展开运算符)

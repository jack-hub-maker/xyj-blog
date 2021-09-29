## axios 的默认配置

在前面的的实例中,我们的 BaseURL 是固定的

1.  事实上在开发中可能很多参数都是固定的
2.  这个时候我们可以进行一些抽取,也可以利用 axios 的默认配置

```js
axios.defaults.baseURL = "http://httpbin.org";
axios.defaults.timeout = 2000;
axios.get("/get").then((res) => {
  console.log(res);
});
axios.post("/post").then((res) => {
  console.log(res);
});
```

这里列举了两个比较常用的全局配置,具体还有很多配置,如果开发有需求的话可以去查一下文档

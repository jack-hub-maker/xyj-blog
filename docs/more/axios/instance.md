当我们的项目越来越大,业务也越来越复杂的时候,往往单独一个全局实例是无法满足我们的需求的,这个时候就可以单独创建一个新的实例

创建对应的 axios 的实例

```js
const instance = axios.create({
  baseURL: "http://httpbin.org",
  timeout: 3000,
});
instance({
  url: "/ip",
}).then((res) => {
  console.log(res);
});
```

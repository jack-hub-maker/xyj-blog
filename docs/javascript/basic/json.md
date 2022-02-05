# JSON

## 一、json 的由来

在目前的开发中，JSON 是一种非常重要的**数据格式**，它并不是编程语言，而是一种可以在服务器和客户端之间传输的数据格式。

JSON 的全称是 JavaScript Object Notation（JavaScript 对象符号）：

- JSON 是由 Douglas Crockford 构想和设计的一种轻量级资料交换格式，算是 JavaScript 的一个子集；
- 但是虽然 JSON 被提出来的时候是主要应用 JavaScript 中，但是目前已经独立于编程语言，可以在各个编程语言中使用；
- 很多编程语言都实现了将 JSON 转成对应模型的方式；

其他的传输格式：

- `XML`：在早期的网络传输中主要是使用 XML 来进行数据交换的，但是这种格式在解析、传输等各方面都弱于 JSON，所以目前已经很
  少在被使用了；
- `Protobuf`：另外一个在网络传输中目前已经**越来越多**使用的传输格式是 protobuf，但是直到 2021 年的 3.x 版本才支持 JavaScript，所
  以目前在前端使用的较少；

目前 JSON 被使用的场景也越来越多：

- 网络数据的传输 JSON 数据；
- 项目的某些配置文件；
- 非关系型数据库（NoSQL）将 json 作为存储格式；

## 二、 json 的基本语法

JSON 的顶层支持三种类型的值：

- 简单值：数字（Number）、字符串（String，不支持单引号）、布尔类型（Boolean）、null 类型；
- 对象值：由 key、value 组成，key 是字符串类型，并且必须添加双引号，值可以是简单值、对象值、数组值；
- 数组值：数组的值可以是简单值、对象值、数组值；

```json
123
```

```json
{
  "name": "tao",
  "age": 18
}
```

```json
[
  32132,
  {
    "name": "tao",
    "age": "18"
  }
]
```

## 三、JSON 序列化

某些情况下我们希望将 JavaScript 中的复杂类型转化成 JSON 格式的字符串，这样方便对其进行处理：

- 比如我们希望将一个对象保存到 localStorage 中
- 但是如果我们直接存放一个对象，这个对象会被转化成 [object Object] 格式的字符串，并不是我们想要的结果；

```js
const info = {
  name: "tao",
  age: 18,
};

localStorage.setItem("info", info);
```

![](https://gitee.com/itsandy/picgo-img/raw/master/JavaScript/未序列化的对象.png)

## 四、JSON 序列化方法

在 ES5 中引用了 JSON 全局对象，该对象有两个常用的方法：

- stringify 方法：将 JavaScript 类型转成对应的 JSON 字符串；
- parse 方法：解析 JSON 字符串，转回对应的 JavaScript 类型；

那么上面的代码我们可以通过如下的方法来使用：

```js
const info = {
  name: "tao",
  age: 18,
};

// 对象->JSON字符串
const infoString = JSON.stringify(info);
localStorage.setItem("info", infoString);

// JSON字符串->对象
const itemString = localStorage.getItem("info");
const newInfo = JSON.parse(itemString);
console.log(newInfo); // {name: 'tao', age: 18}
```

![](https://gitee.com/itsandy/picgo-img/raw/master/JavaScript/JSON序列化.png)

## 五、Stringify 方法的细节

[JSON.stringify()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) 方法将一个 JavaScript 对象或值转换为 JSON 字符串：

```js
const info = {
  name: "tao",
  age: 18,
};

// 第一个参数value
const infoString = JSON.stringify(info);
console.log(infoString); // {"name":"tao","age":18}

// 第二个参数replacer

// 回调函数（key，value）可以理解为拦截，然后做一些操作
const infoString2 = JSON.stringify(info, (key, value) => {
  if (key === "age") {
    return value + 1;
  }
  return value;
});
console.log(infoString2); // {"name":"tao","age":19}

// 数组（选择需要转换的属性）
const infoString3 = JSON.stringify(info, ["name"]);
console.log(infoString3); // {"name":"tao"}

// 第三个参数space（指定缩进用的空白字符串，美化输出）
const infoString4 = JSON.stringify(info, null, 2);
console.log(infoString4);
/**
 * {
  "name": "tao",
  "age": 18
  }
 */
```

?>如果对象本身包含 toJSON 方法，那么会直接使用 toJSON 方法的结果：

```js
const info = {
  name: "tao",
  age: 18,
  toJSON() {
    return "toString";
  },
};

const infoString = JSON.stringify(info);
console.log(infoString); // "toString"
```

## 六、parse 方法的细节

[JSON.parse()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse) 方法用来解析 JSON 字符串，构造由字符串描述的 JavaScript 值或对象。

```js
const info = {
  name: "tao",
  age: 18,
};

const infoString = JSON.stringify(info);

// 第一个参数text
const newInfo = JSON.parse(infoString);
console.log(newInfo); // {name: 'tao', age: 18}

// 第二个参数reviver（回调函数(key,value)拦截）
const newInfo2 = JSON.parse(infoString, (key, value) => {
  if (key === "name") {
    return (name = "sandy");
  }
  return value;
});
console.log(newInfo2); // {name: 'sandy', age: 18}
```

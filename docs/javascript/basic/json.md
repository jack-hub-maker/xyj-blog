# JSON

## 一、json的由来

在目前的开发中，JSON是一种非常重要的**数据格式**，它并不是编程语言，而是一种可以在服务器和客户端之间传输的数据格式。

JSON的全称是JavaScript Object Notation（JavaScript对象符号）：
- JSON是由Douglas Crockford构想和设计的一种轻量级资料交换格式，算是JavaScript的一个子集；
- 但是虽然JSON被提出来的时候是主要应用JavaScript中，但是目前已经独立于编程语言，可以在各个编程语言中使用；
- 很多编程语言都实现了将JSON转成对应模型的方式；

其他的传输格式：
- `XML`：在早期的网络传输中主要是使用XML来进行数据交换的，但是这种格式在解析、传输等各方面都弱于JSON，所以目前已经很
少在被使用了；
- `Protobuf`：另外一个在网络传输中目前已经**越来越多**使用的传输格式是protobuf，但是直到2021年的3.x版本才支持JavaScript，所
以目前在前端使用的较少；


目前JSON被使用的场景也越来越多：
- 网络数据的传输JSON数据；
- 项目的某些配置文件；
- 非关系型数据库（NoSQL）将json作为存储格式；

## 二、 json的基本语法

JSON的顶层支持三种类型的值：
- 简单值：数字（Number）、字符串（String，不支持单引号）、布尔类型（Boolean）、null类型；
- 对象值：由key、value组成，key是字符串类型，并且必须添加双引号，值可以是简单值、对象值、数组值；
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

## 三、JSON序列化

某些情况下我们希望将JavaScript中的复杂类型转化成JSON格式的字符串，这样方便对其进行处理：
- 比如我们希望将一个对象保存到localStorage中（下一节会讲）；
- 但是如果我们直接存放一个对象，这个对象会被转化成 [object Object] 格式的字符串，并不是我们想要的结果；

```js
const info = {
  name: 'tao',
  age: 18
}

localStorage.setItem('info', info)
```

![](https://gitee.com/itsandy/picgo-img/raw/master/JavaScript/未序列化的对象.png)

## 四、JSON序列化方法

在ES5中引用了JSON全局对象，该对象有两个常用的方法：
- stringify方法：将JavaScript类型转成对应的JSON字符串；
- parse方法：解析JSON字符串，转回对应的JavaScript类型；

那么上面的代码我们可以通过如下的方法来使用：

```js
const info = {
  name: 'tao',
  age: 18
}

// 对象->JSON字符串
const infoString = JSON.stringify(info)
localStorage.setItem('info', infoString)

// JSON字符串->对象
const itemString = localStorage.getItem('info')
const newInfo = JSON.parse(itemString)
console.log(newInfo); // {name: 'tao', age: 18}
```

![](https://gitee.com/itsandy/picgo-img/raw/master/JavaScript/JSON序列化.png)


## 五、Stringify方法的细节

JSON.stringify() 方法将一个 JavaScript 对象或值转换为 JSON 字符串：

详情：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify

```js
const info = {
  name: 'tao',
  age: 18
}

// 第一个参数value
const infoString = JSON.stringify(info)
console.log(infoString); // {"name":"tao","age":18}

// 第二个参数replacer

// 回调函数（key，value）可以理解为拦截，然后做一些操作
const infoString2 = JSON.stringify(info, (key, value) => {
  if (key === 'age') {
    return value + 1
  }
  return value
})
console.log(infoString2); // {"name":"tao","age":19}

// 数组（选择需要转换的属性）
const infoString3 = JSON.stringify(info, ['name'])
console.log(infoString3); // {"name":"tao"}

// 第三个参数space（指定缩进用的空白字符串，美化输出）
const infoString4 = JSON.stringify(info, null, 2)
console.log(infoString4);
/**
 * {
  "name": "tao",
  "age": 18
  }
 */
```

!>如果对象本身包含toJSON方法，那么会直接使用toJSON方法的结果：

```js
const info = {
  name: 'tao',
  age: 18,
  toJSON() {
    return 'toString'
  }
}

const infoString = JSON.stringify(info)
console.log(infoString);  // "toString"
```

## 六、parse方法的细节

JSON.parse() 方法用来解析JSON字符串，构造由字符串描述的JavaScript值或对象。

详情：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse

```js
const info = {
  name: 'tao',
  age: 18,
}

const infoString = JSON.stringify(info)

// 第一个参数text
const newInfo = JSON.parse(infoString)
console.log(newInfo); // {name: 'tao', age: 18}

// 第二个参数reviver（回调函数(key,value)拦截）
const newInfo2 = JSON.parse(infoString, (key, value) => {
  if (key === 'name') {
    return name = 'sandy'
  }
  return value
})
console.log(newInfo2); // {name: 'sandy', age: 18}
```

## 七、使用JSON序列化深拷贝

!>不了解深拷贝的话，这里可以跳过，后续章节会详细介绍浅拷贝和深拷贝

另外我们生成的新对象和之前的对象并不是同一个对象：
- 相当于是进行了一次深拷贝；

```js
const info = {
  name: 'tao',
  age: 18,
  friends: ['zs', 'ls', 'ww']
}

const newInfo = JSON.parse(JSON.stringify(info))
console.log(newInfo); // {name: 'tao', age: 18, friends: Array(3)}

newInfo.friends = ['zs']
console.log(info.friends); // ['zs', 'ls', 'ww']
```

!>注意：这种方法它对函数是无能为力的(因为JSON里面是没有函数的，所以并不会对函数进行处理)

```js
const info = {
  name: 'tao',
  age: 18,
  friends: ['zs', 'ls', 'ww'],
  foo() {
    console.log('foo');
  }
}

const newInfo = JSON.parse(JSON.stringify(info))
console.log(newInfo); // {name: 'tao', age: 18, friends: Array(3)}
```

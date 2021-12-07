# 浅拷贝和深拷贝

## 一、认识浅拷贝和深拷贝

浅拷贝是创建一个新对象，这个对象有着原始对象属性值的一份精确拷贝。如果属性是基本类型，拷贝的就是基本类型的值，如果属性是引用类型，拷贝的就是内存地址 ，所以**如果其中一个对象改变了这个地址，就会影响到另一个对象**。

![](https://gitee.com/itsandy/picgo-img/raw/master/JavaScript/浅拷贝.webp)

深拷贝是将一个对象从内存中完整的拷贝一份出来,从堆内存中开辟一个新的区域存放新对象,且**修改新对象不会影响原对象**。

![](https://gitee.com/itsandy/picgo-img/raw/master/JavaScript/深拷贝.webp)

## 二、浅拷贝的实现方式

### 2.1 Object.assign

[Object.assign()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) 方法用于将所有可枚举属性的值从一个或多个源对象分配到目标对象。它将返回目标对象。

```js
const obj = {
  name: "tao",
  age: 18,
  friends: {
    name: "zs",
    friends: {
      name: "ls",
    },
  },
};

const newObj = Object.assign(obj);
obj.friends.friends.name = "sandy";
console.log(newObj.friends.friends.name); // sandy
```

### 2.2 展开运算符

[展开语法](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Spread_syntax)(Spread syntax), 可以在函数调用/数组构造时, 将数组表达式或者 string 在语法层面展开；还可以在构造字面量对象时, 将对象表达式按 key-value 的方式展开。

```js
const obj = {
  name: "tao",
  age: 18,
  friends: {
    name: "zs",
    friends: {
      name: "ls",
    },
  },
};
const newObj = { ...obj };
obj.friends.friends.name = "sandy";
console.log(newObj.friends.friends.name); // sandy
```

### 2.3 Array.prototype.concat

[concat](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/concat)() 方法用于合并两个或多个数组。此方法不会更改现有数组，而是返回一个新数组。

```js
const names = ["zs", "ls", "ww", { name: "zl" }];
const newNames = names.concat();
names[3].name = "snady";
console.log(newNames[3].name); // sandy
```

### 2.4 Array.prototype.slice()

[slice](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)() 方法返回一个新的数组对象，这一对象是一个由 begin 和 end 决定的原数组的浅拷贝（包括 begin，不包括 end）。原始数组不会被改变。

```js
const names = ["zs", "ls", "ww", { name: "zl" }];
const newNames = names.slice();
names[3].name = "snady";
console.log(newNames[3].name); // sandy
```

### 2.5 lodash 库 clone 方法

lodash 库也为我们提供了实现浅拷贝的函数[clone](https://lodash.com/docs/4.17.15#clone)

```html
<script src="https://cdn.jsdelivr.net/npm/lodash@4.17.21/lodash.min.js"></script>
<script>
  const obj = {
    name: "tao",
    age: 18,
    friends: {
      name: "zs",
      friends: {
        name: "ls",
      },
    },
  };

  const newObj = _.clone(obj);
  obj.friends.friends.name = "sandy";
  console.log(newObj.friends.friends.name); // sandy
</script>
```

## 三、深拷贝的实现方式

总而言之，浅拷贝只复制指向某个对象的指针，而不复制对象本身，**新旧对象还是共享同一块内存**。但深拷贝会另外创造一个一模一样的对象，**新对象跟原对象不共享内存**，修改新对象不会改到原对象。

前面我们已经学习了对象相互赋值的一些关系，分别包括：

- 引入的赋值：指向同一个对象，相互之间会影响；
- 对象的浅拷贝：只是浅层的拷贝，内部引入对象时，依然会相互影响；
- 对象的深拷贝：两个对象不再有任何关系，不会相互影响；

### 3.1 JSON 序列化

```js
const obj = {
  name: "tao",
  age: 18,
  friends: {
    name: "zs",
    friends: {
      name: "ls",
    },
  },
};

const newObj = JSON.parse(JSON.stringify(obj));
obj.friends.friends.name = "sandy";
console.log(newObj.friends.friends.name); // ls
```

这种深拷贝的方式其实对于函数、Symbol 等是无法处理的

```js
const obj = {
  name: "tao",
  age: 18,
  friends: {
    name: "zs",
    friends: {
      name: "ls",
    },
  },
  foo() {
    console.log("foo");
  },
};

const newObj = JSON.parse(JSON.stringify(obj));
console.log(newObj.foo); // undefined
```

并且如果存在对象的循环引用，也会报错的；

```js
const obj = {
  name: "tao",
  age: 18,
  friends: {
    name: "zs",
    friends: {
      name: "ls",
    },
  },
};

obj.info = obj;
const newObj = JSON.parse(JSON.stringify(obj));
// Converting circular structure to JSON
```

### 3.2 lodash 库 cloneDeep 方法

lodash 库也为我们提供了实现深拷贝的函数[cloneDeep](https://lodash.com/docs/4.17.15#cloneDeep)

```html
<script src="https://cdn.jsdelivr.net/npm/lodash@4.17.21/lodash.min.js"></script>
<script>
  const obj = {
    name: "tao",
    age: 18,
    friends: {
      name: "zs",
      friends: {
        name: "ls",
      },
    },
  };

  obj.info = obj;
  const newObj = _.cloneDeep(obj);
  obj.friends.friends.name = "sandy";
  console.log(newObj.friends.friends.name); // ls
</script>
```

### 3.3 自定义深拷贝函数

在工作的时候我们可能不会自己来实现深拷贝函数，而是用一些第三方的库（比如 lodash），但是在学习阶段，我们最好还是自己来手写，能够帮助我们更好的理解，在面试的时候如果要求手写的话也是难不倒我们的

#### 3.3.1 实现深拷贝的基本功能

```js
const obj = {
  name: "tao",
  age: 18,
  friends: {
    name: "zs",
    friends: {
      name: "ls",
    },
  },
};

function isObject(value) {
  const valueType = typeof value;
  return (
    valueType !== null && (valueType === "function" || valueType === "object")
  );
}

function cloneDeep(obj) {
  // 判断传入的值是否是一个对象，不是对象就返回出去
  if (!isObject(obj)) {
    return obj;
  }

  const newObj = {};

  // 递归调用（实现深拷贝）
  for (const key in obj) {
    newObj[key] = cloneDeep(obj[key]);
  }

  return newObj;
}

const newObj = cloneDeep(obj);

console.log(obj === newObj);
obj.friends.friends.name = "3232";
console.log(newObj);
```

#### 3.3.2 其他数据类型的值进行处理

```js
const s1 = Symbol("aaa");
const s2 = Symbol("bbb");

const obj = {
  name: "tao",
  age: 18,
  friends: {
    name: "zs",
    friends: {
      name: "ls",
    },
  },
  // 数组
  names: ["zs", "ls", "ww"],
  // 函数
  foo() {
    console.log("foo");
  },
  // Symbol作为key和value
  s1: s1,
  [s2]: s2,
  // Set和Map
  set: new Set(["zs", "ls", "ww"]),
  map: new Map([
    ["aaa", "bbb"],
    ["ccc", "ddd"],
  ]),
};

function isObject(value) {
  const valueType = typeof value;
  return (
    valueType !== null && (valueType === "function" || valueType === "object")
  );
}

function cloneDeep(obj) {
  // 判断如果是Symbol类型的value
  if (typeof obj === "symbol") {
    return Symbol(obj.description);
  }

  // 判断如果是Set类型的value（Set和Map的话放在对象里的话是用的非常非常少的，这里就简单处理一下（浅拷贝））
  if (obj instanceof Set) {
    return new Set([...obj]);
  }

  // 判断如果是Map类型的value
  if (obj instanceof Map) {
    return new Map([...obj]);
  }

  // 判断如果不是对象类型
  if (!isObject(obj)) {
    return obj;
  }

  // 判断是一个数组还是一个对象
  const newObj = Array.isArray(obj) ? [] : {};

  // 判断如果是函数类型（这里有个问题，我们的函数要不要重新创建一份新的喃？可以拷贝但没必要，我们一般使用函数可能只是为了实现代码的复用）
  if (typeof obj === "function") {
    return obj;
  }

  // 递归调用（实现深拷贝）
  for (const key in obj) {
    newObj[key] = cloneDeep(obj[key]);
  }

  // 判断如果Symbol类型的key
  const symbolKeys = Object.getOwnPropertySymbols(obj);
  for (const skey of symbolKeys) {
    newObj[skey] = cloneDeep(obj[skey]);
  }

  return newObj;
}

const newObj = cloneDeep(obj);

console.log(obj === newObj);
console.log(newObj);
```

#### 3.3.3 对循环引用的处理

```js
const s1 = Symbol("aaa");
const s2 = Symbol("bbb");

const obj = {
  name: "tao",
  age: 18,
  friends: {
    name: "zs",
    friends: {
      name: "ls",
    },
  },
  // 数组
  names: ["zs", "ls", "ww"],
  // 函数
  foo() {
    console.log("foo");
  },
  // Symbol作为key和value
  s1: s1,
  [s2]: s2,
  // Set和Map
  set: new Set(["aaa", "bbb", "ccc"]),
  map: new Map([
    ["zs", "32"],
    ["ls", "21"],
  ]),
};

function isObject(value) {
  const valueType = typeof value;
  return (
    valueType !== null && (valueType === "function" || valueType === "object")
  );
}

function cloneDeep(obj, map = new WeakMap()) {
  // 判断如果是Symbol类型的value
  if (typeof obj === "symbol") {
    return Symbol(obj.description);
  }

  // 判断如果是Set类型的value（Set和Map的话放在对象里的话是用的非常非常少的，这里就简单处理一下）
  if (obj instanceof Set) {
    return new Set([...obj]);
  }

  // 判断如果是Map类型的value
  if (obj instanceof Map) {
    return new Map([...obj]);
  }

  // 判断如果不是对象类型
  if (!isObject(obj)) {
    return obj;
  }

  // 判断如果是函数类型（这里有个问题，我们的函数要不要重新创建一份新的喃？可以拷贝但没必要，我们一般使用函数可能只是为了实现代码的复用）
  if (typeof obj === "function") {
    return obj;
  }

  // 处理循环引用的逻辑
  if (map.has(obj)) {
    return map.get(obj);
  }

  // 判断是一个数组还是一个对象
  const newObj = Array.isArray(obj) ? [] : {};
  map.set(obj, newObj);
  // 递归调用（实现深拷贝）
  for (const key in obj) {
    newObj[key] = cloneDeep(obj[key], map);
  }

  // 判断如果Symbol类型的key
  const symbolKeys = Object.getOwnPropertySymbols(obj);
  for (const skey of symbolKeys) {
    newObj[skey] = cloneDeep(obj[skey], map);
  }

  return newObj;
}

obj.info = obj;
const newObj = cloneDeep(obj);

console.log(obj === newObj);
console.log(newObj);
```

## 四、参考文章

- [浅拷贝与深拷贝](https://juejin.cn/post/6844904197595332622#heading-14)

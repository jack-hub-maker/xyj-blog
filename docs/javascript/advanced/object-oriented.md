## 面向对象是现实的抽象方式

对象是 JavaScript 中一个非常重要的概念，这是因为对象可以将多个相关联的数据封装到一起，更好的描述一个事物：

- `比如我们可以描述一辆车`：Car，具有颜色（color）、速度（speed）、品牌（brand）、价格（price），行驶（travel）等
  等；
- `比如我们可以描述一个人`：Person，具有姓名（name）、年龄（age）、身高（height），吃东西（eat）、跑步（run）
  等等；

用`对象来描述事物`，更有利于我们`将现实的事物`，抽离成代码中`某个数据结构`：

- 所以有一些编程语言就是纯面向对象的编程语言，比如 Java；
- 你在实现任何现实抽象时都需要先创建一个类，根据类再去创建对象；

## JavaScript 的面向对象

JavaScript 其实支持多种编程范式的，包括函数式编程和面向对象编程：

- JavaScript 中的对象被设计成一组`属性的无序集合`，像是一个`哈希表`，有 key 和 value 组成；
- `key是一个标识符名称`，`value可以是任意类型`，也可以是`其他对象或者函数类型`；
- 如果值`是一个函数`，那么我们可以称之为是`对象的方法`；

如何创建一个对象呢？

- 早期使用创建对象的方式最多的是`使用Object类`，并且`使用new关键字`来创建一个对象：
  - 这是因为早期很多 JavaScript 开发者是从 Java 过来的，它们也更习惯于 Java 中通过 new 的方式创建一个对象；
- 后来很多开发者为了方便起见，都是直接`通过字面量的形式来创建对象`：
  - 这种形式看起来更加的简洁，并且对象和属性之间的内聚性也更强，所以这种方式后来就流行了起来；

## 创建对象的两种方式

```js
// new
var person = new Object();
person.name = "tao";
person.age = 19;
person.run = function () {
  console.log(this.name + "跑步");
};

console.log(person);

// 字面量
var info = {
  name: "tao",
  age: 19,
  run: function () {
    console.log(this.name + "跑步");
  },
};
console.log(info);
```

## 对属性操作的控制

在前面我们的属性都是`直接定义在对象内部`，或者`直接添加到对象内部`的：

- 但是这样来做的时候我们就`不能对这个属性进行一些限制`：比如`这个属性是否是可以通过 delete 删除的`？这个
  属性`是否在 for-in 遍历的时候被遍历出来`呢？

如果我们想要对`一个属性进行比较精准的操作控制`，那么我们就可以使用`属性描述符`。

- 通过属性描述符`可以精准的添加或修改对象的属性`；
- 属性描述符需要使用 `Object.defineProperty` 来对属性进行添加或者修改；

## Object.defineProperty

详情：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty

### 属性描述符分类

属性描述符的类型有两种：

- `数据属性`（Data Properties）描述符（Descriptor）；
- `存取属性`（Accessor 访问器 Properties）描述符（Descriptor）；

> 数据属性描述符

```js
// name和age虽然没有属性描述符来定义，但是它们也是具有对应的特性（默认都为true）
var obj = {
  name: "tao",
  age: 18,
};

Object.defineProperty(obj, "height", {
  // 值（默认为 undefined）
  value: "1.88",
  // 是否可配置的（默认为 false）
  configurable: false,
  // 是否可枚举（默认为 false）
  enumerable: false,
  // 是否可写（默认为 false）
  writable: false,
});

for (var key in obj) {
  console.log(key);
}

Object.keys(obj);

obj.height = "123";
console.log(obj.height);
```

> 存取属性描述符

```js
var obj = {
  name: "tao",
  age: 18,
  _height: "1.88",
};

// 存取属性描述符
// 1.隐藏某一个私有属性希望直接被外界操作
// 2.如果我们希望截获某一个属性它访问和设置值的过程时，也会使用存储属性描述符
Object.defineProperty(obj, "height", {
  configurable: true,
  enumerable: true,
  set: function (value) {
    foo();
    this._height = value;
  },
  get: function () {
    bar();
    return this._height;
  },
});

function foo() {
  console.log("设置了height属性的值");
}

function bar() {
  console.log("获取了height属性的值");
}

obj.height = "123";
console.log(obj.height);
```

这里提到了这个东西，就简单说一下我对 vue2 响应式原理的理解

比如

data(){return{name:'tao',age:19}}

首先会对 data 中返回的对象的 key 遍历，然后通过 Object.defineProperty 截获它的 get 和 set，截获到了我就可以做到如果某个地方对 name 或者 age 有依赖，我就会收集对应的依赖，当我有一天设置新的值的时候，我就从那些依赖里面拿到所有该执行的方法重新执行一遍，就可以当这些属性发生改变的时候让它该更新的地方就更新

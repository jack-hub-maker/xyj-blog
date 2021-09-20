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

## JS 创建对象的方案

### 创建对象的两种方式

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

### 对属性操作的控制

在前面我们的属性都是`直接定义在对象内部`，或者`直接添加到对象内部`的：

- 但是这样来做的时候我们就`不能对这个属性进行一些限制`：比如`这个属性是否是可以通过 delete 删除的`？这个
  属性`是否在 for-in 遍历的时候被遍历出来`呢？

如果我们想要对`一个属性进行比较精准的操作控制`，那么我们就可以使用`属性描述符`。

- 通过属性描述符`可以精准的添加或修改对象的属性`；
- 属性描述符需要使用 `Object.defineProperty` 来对属性进行添加或者修改；

#### Object.defineProperty

详情：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty

### 属性描述符分类

属性描述符的类型有两种：

- `数据属性`（Data Properties）描述符（Descriptor）；
- `存取属性`（Accessor 访问器 Properties）描述符（Descriptor）；

### 数据属性描述符

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

### 存取属性描述符

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

#### Object.defineProperties

定义多个属性描述符

```js
var obj = {
  name: "tao",
  _age: 18,
};

Object.defineProperties(obj, {
  age: {
    configurable: true,
    enumerable: true,
    set(value) {
      this.age = value;
    },
    get() {
      return this.age;
    },
  },
});

obj._age = "20";
console.log(obj._age);
```

其实还可以这样写

```js
var obj = {
  name: "tao",
  _age: 18,
  set age(value) {
    this._age = value;
  },
  get age() {
    return this._age;
  },
};

obj._age = "20";
console.log(obj._age);
```

这其实是等价的，只不过上面的写法能够更改属性描述符，下面的写法比较简洁

这里补充一个小的知识点

> [!tip]
> 关于私有属性的概念
> 在 JavaScript 中一般定义私有属性会在属性的前面加上`_`，这并不是严格意义上的私有，只是在 JS 社区里的一种`规范`，当 JS 程序员看到属性前面有个`_`,就会把他视为是一个私有属性，所以在上面的案例中，我们是可以获取私有属性的

### 其它方法补充

#### 获取对象的属性描述符：

- getOwnPropertyDescriptor
- getOwnPropertyDescriptors

#### 禁止对象继续添加新的属性

详情：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/preventExtensions

```js
var obj = {
  name: "tao",
  age: 18,
};

Object.preventExtensions(obj);
obj.height = 1.88;
console.log(obj);
```

#### 禁止对象配置/删除里面的属性

详情：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/seal

```js
var obj = {
  name: "tao",
  age: 18,
};

Object.seal(obj);

delete obj.name;
console.log(obj);
```

#### 让属性不可以修改

详情：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze

```js
var obj = {
  name: "tao",
  age: 18,
};

Object.freeze(obj);

obj.name = "sandy";
console.log(obj.name);
```

## 创建多个对象的方案

如果我们现在希望创建一系列的对象：比如 Person 对象

- 包括张三、李四、王五、李雷等等，他们的信息各不相同；
- 那么采用什么方式来创建比较好呢？

目前我们已经学习了两种方式：

- new Object 方式；
- 字面量创建的方式；

这种方式有一个很大的弊端：创建同样的对象时，需要编写重复的代码；

### 工厂模式

如果我们想创建一次性多个具有相同性的对象，这个我们会联想到工厂模式

工厂模式其实是一种常见的设计模式；

通常我们会有一个工厂方法，通过该工厂方法我们可以产生想要的对象；

```js
function person(name, age) {
  var info = {};
  info.name = name;
  info.age = age;
  info.say = function () {
    console.log(this.name + "say");
  };

  return info;
}

var p1 = person("tao", 18);
var p2 = person("sandy", 21);
```

### 认识构造函数

工厂方法创建对象有一个比较大的问题：我们在打印对象时，对象的类型都是 Object 类型

- 但是从某些角度来说，这些对象应该有一个他们共同的类型；
- 下面我们来看一下另外一种模式：构造函数的方式；

我们先理解什么是构造函数？

- 构造函数也称之为构造器（constructor），通常是我们在创建对象时会调用的函数；
- 在其他面向的编程语言里面，构造函数是存在于类中的一个方法，称之为构造方法；
- 但是 JavaScript 中的构造函数有点不太一样；

JavaScript 中的构造函数是怎么样的？

- 构造函数也是一个普通的函数，从表现形式来说，和千千万万个普通的函数没有任何区别；
- 那么如果这么一个普通的函数被使用 new 操作符来调用了，那么这个函数就称之为是一个构造函数；
- 那么被 new 调用有什么特殊的呢？

### new 操作符调用的作用

如果一个函数被使用 new 操作符调用了，那么它会执行如下操作：

1.  在内存中创建一个新的对象（空对象）；
2.  这个对象内部的`prototype`属性会被赋值为该构造函数的 prototype 属性；（后面详细讲）；
3.  构造函数内部的 this，会指向创建出来的新对象；
4.  执行函数的内部代码（函数体代码）；
5.  如果构造函数没有返回非空对象，则返回创建出来的新对象；

```js
function foo() {
  console.log("foo");
}

var p = new foo();
console.log(p);
```

![image.png](https://img13.360buyimg.com/ddimg/jfs/t1/207788/2/1338/3727/6146fa8dEf384db2a/8e83019d490d17d7.png)

### 构造函数

我们来通过构造函数实现一下创建一次性多个具有相同性的对象

```js
// 构造函数的首字母为大写(社区约定俗成的规范)
function Person(name, age) {
  this.name = name;
  this.age = age;
  this.say = function () {
    console.log(this.name + "say");
  };
}

var p1 = new Person("tao", 18);
var p2 = new Person("sandy", 21);
console.log(p1, p2);
```

![image.png](https://img13.360buyimg.com/ddimg/jfs/t1/108850/10/17839/4012/6146fc97E03155467/689ea3baac88fe90.png)

这个构造函数可以确保我们的对象是有 Person 的类型的（实际是 constructor 的属性，这个我们后续再探讨）；

但是构造函数就没有缺点了吗？

构造函数也是有缺点的，它在于我们需要为每个对象的函数去创建一个函数对象实例；

### 认识对象的原型

JavaScript 当中每个对象都有一个特殊的内置属性 [[prototype]]，这个特殊的对象可以指向另外一个对象。

那我怎么获取这个对象的原型喃？

- 通过对象的 \***\*proto\*\***（隐式原型） 属性可以获取到（早期 ECMA 没有规范如何查看[[prototype]],于是有部分浏览器自己提供了这个方法来让我们查看）；
- 方式二：通过 Object.getPrototypeOf 方法可以获取到；

```js
var info = { name: "tao", age: 18 };
console.log(info.__proto__);
console.log(Object.getPrototypeOf(info));
```

那么这个对象有什么用呢？

- 当我们通过引用对象的属性 key 来获取一个 value 时，它会触发 [[Get]]的操作；
- 这个操作会首先检查该属性是否有对应的属性，如果有的话就使用它；
- 如果对象中没有改属性，那么会访问对象[[prototype]]内置属性指向的对象上的属性；

```js
var info = { name: "tao", age: 18 };

info.__proto__.height = 1.88;

console.log(info.height);
```

这个也设计到原型链，后续会讲到

### 函数的原型

广义的说函数也是一个对象

作为对象来说它也有[[prototype]]隐式原型

```js
function foo() {}
console.log(foo.__proto__); // {}
```

但是从具体来说，函数它自身有一个显式原型 prototype

```js
function foo() {}
console.log(foo.prototype); // {}
```

我们前面讲过 new 关键字的步骤如下：

1.  在内存中创建一个新的对象（空对象）；
2.  这个对象内部的[[prototype]]属性会被赋值为该构造函数的 prototype 属性

那么也就意味着我们通过 Person 构造函数创建出来的所有对象的[[prototype]]属性都指向 Person.prototype：

```js
function Foo() {}
var p1 = new foo();

// 上面的执行相对于下面的操作
var moni = {};
moni.__proto__ = Foo.prototype;
```

```js
function Foo() {}
var p1 = new Foo();
var p2 = new Foo();

console.log(p1.__proto__ === p2.__proto__); // true
console.log(p1.__proto__ === Foo.prototype); // true
```

创建内存的表现

```js
function Foo() {}
var p1 = new Foo();
var p2 = new Foo();
```

解析上面的代码在内存中的表现

![image.png](https://img11.360buyimg.com/ddimg/jfs/t1/208927/17/1361/54339/61483e06Eaad4cbc4/adcbd69a822d5846.png)

这里我简单画了具体的内存，就没有画其它的 GO，VO 那些

主要是说当我们创建了一个函数，函数中的 prototype 就会指向函数的原型对象，然后通过 new 操作符创造出来的对象中的**proto**也指向的是函数的原型对象

听懂了，我们来看下面的代码

```js
function Foo() {}
var p1 = new Foo();
var p2 = new Foo();

// p1.name = 'tao'
// p1.__proto__.name = 'sandy'
// Foo.prototype.name = 'zm'
// p2.__proto__.name = 'ymy'

console.log(p1.name);
```

通过上面四种方式都可以打印 p1.name

本质上我们修改 p1，p2 的原型，实际上最终修改的是 Foo 构造函数对象的原型，因为 p1，p2 指向的是构造函数对象的原型

### constructor 属性

事实上每个原型对象上都有一个 constructor 属性

```js
function foo() {}
console.log(foo.prototype.constructor); // foo
```

默认情况下原型上都会添加一个属性叫做 constructor，这个 constructor 指向当前的函数对象；

为什么前面我们在打印 foo.prototype 的时候没有看到 constructor 属性，那是因为默认原型对象上的 constructor 的 enumerable 为 false 是不可枚举的

```js
function foo() {}

console.log(Object.getOwnPropertyDescriptor(foo, "prototype")); // { value: {}, writable: true, enumerable: false, configurable: false }
```

在内存中表现就是这样的

![image.png](https://img10.360buyimg.com/ddimg/jfs/t1/108800/29/17643/22118/61484ed9E322266bd/9321dac346b9a457.png)

这就说明我们可以这样写代码

```js
console.log(
  foo.prototype.constructor.prototype.constructor.prototype.constructor
); // foo
```

### 重写原型对象

我们可以在原型上添加自己的属性

```js
function foo() {}

foo.prototype.name = "tao";
foo.prototype.age = 18;
foo.prototype.height = 1.88;
```

但是假如这里需要添加的属性非常多，我们每次前面都要写这个 foo.prototype 就感觉非常的冗余

所以我们也会看到这种代码

```js
foo.prototype = {
  name: "tao",
  age: 18,
  height: 1.88,
};
```

这段代码表示，我们 foo.prototype 指向了一个新的对象，而不再指向原来的原型对象

![image.png](https://img14.360buyimg.com/ddimg/jfs/t1/94745/2/19554/43569/6148509fE66730abf/b82e4035ab689426.png)

因为原型对象我们根据前面讲的标记清除的算法，没有东西指向你原型对象了，那么原型对象是会被回收掉的

这个时候会想那我的 constructor 属性怎么办喃？这个时候我们就可以手动给原型上添加 constructor 属性

### 原型对象的 constructor

```js
function foo() {}

foo.prototype = {
  constructor: foo,
  name: "tao",
  age: 18,
  height: 1.88,
};
```

这种方法虽然可以，但说过这样直接添加的话 constructor 的 Enumerable 属性描述符就变成了 true

默认情况下, 原生的 constructor 属性是不可枚举的.

如果希望解决这个问题, 就可以使用我们前面介绍的 Object.defineProperty()函数了.

```js
function foo() {}

foo.prototype = {
  name: "tao",
  age: 18,
  height: 1.88,
};

Object.defineProperty(foo.prototype, "constructor", {
  valeu: foo,
  enumerable: false,
});
```

### 构造函数和原型组合

接下来我们就可以回到最初的问题了，我们在上一个构造函数的方式创建对象时，有一个弊端：会创建出重复的函数，比如 running、eating 这些函数

那么有没有办法让所有的对象去共享这些函数呢?

可以，将这些函数放到 Person.prototype 的对象上即可；

你可能会这样写，我把所有的东西都放在原型上，然后不就不会重复的吗

```js
function Person(name, age) {
  Person.prototype.name = name;
  Person.prototype.age = age;
  Person.prototype.say = function () {
    console.log(this.name + "在说话");
  };
}

var p1 = new Person("tao", 18);
var p2 = new Person("sandy", 21);
console.log(p1);
```

这样对吗，不对！很明显这里打印 p1.name 的值是 sandy

因为前面说过，当我们通过.的方式获取值的时候，会触发对应的 get 方法，自己的对象中找不到，就会去原型上找

在 p2 定义的时候把原型对象上的 name 修改为了 sandy，所以 p1 去原型上找的 name 就是 sandy

所以一般我们会把重复的函数放在原型对象上

```js
function Person(name, age) {
  this.name = name;
  this.age = age;
}

Person.prototype.say = function () {
  console.log(this.name + "在说话");
};

var p1 = new Person("tao", 18);
var p2 = new Person("sandy", 21);
```

有人就会好奇在外面定义 say 函数会不会出错啊，这就是考验到 this 的问题了，前面我们说了 this 是动态绑定的，this 的绑定和编写的位置是没有关系的，是跟调用位置有关

这也用到了我们前面所讲的内容，所以我们都是循序渐进的

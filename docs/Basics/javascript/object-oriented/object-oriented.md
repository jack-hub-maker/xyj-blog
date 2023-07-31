# 面向对象
## 一、理解面向对象

对象是 JavaScript 中一个非常重要的概念，这是因为对象可以将多个相关联的数据封装到一起，更好的描述一个事物：

- `比如我们可以描述一辆车`：Car，具有颜色（color）、速度（speed）、品牌（brand）、价格（price），行驶（travel）等
  等；
- `比如我们可以描述一个人`：Person，具有姓名（name）、年龄（age）、身高（height），吃东西（eat）、跑步（run）
  等等；

用`对象来描述事物`，更有利于我们`将现实的事物`，抽离成代码中`某个数据结构`：

- 所以有一些编程语言就是纯面向对象的编程语言，比如 Java；
- 你在实现任何现实抽象时都需要先创建一个类，根据类再去创建对象；

## 二、JavaScript 的面向对象

JavaScript 其实支持多种编程范式的，包括函数式编程和面向对象编程：

- JavaScript 中的对象被设计成一组`属性的无序集合`，像是一个`哈希表`，有 key 和 value 组成；
- `key是一个标识符名称`，`value可以是任意类型`，也可以是`其他对象或者函数类型`；
- 如果值`是一个函数`，那么我们可以称之为是`对象的方法`；

如何创建一个对象呢？

- 早期使用创建对象的方式最多的是`使用Object类`，并且`使用new关键字`来创建一个对象：
  - 这是因为早期很多 JavaScript 开发者是从 Java 过来的，它们也更习惯于 Java 中通过 new 的方式创建一个对象；
- 后来很多开发者为了方便起见，都是直接`通过字面量的形式来创建对象`：
  - 这种形式看起来更加的简洁，并且对象和属性之间的内聚性也更强，所以这种方式后来就流行了起来；

## 三、JS创建对象的方案

### 3.1 创建对象的两种方式

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

### 3.2 对属性操作的控制

在前面我们的属性都是`直接定义在对象内部`，或者`直接添加到对象内部`的：

- 但是这样来做的时候我们就`不能对这个属性进行一些限制`：比如`这个属性是否是可以通过 delete 删除的`？这个
  属性`是否在 for-in 遍历的时候被遍历出来`呢？

如果我们想要对`一个属性进行比较精准的操作控制`，那么我们就可以使用`属性描述符`。

- 通过属性描述符`可以精准的添加或修改对象的属性`；
- 属性描述符需要使用 `Object.defineProperty` 来对属性进行添加或者修改；


详情：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty


属性描述符的类型有两种：

- `数据属性`（Data Properties）描述符（Descriptor）；
- `存取属性`（Accessor 访问器 Properties）描述符（Descriptor）；

#### 3.2.1 数据属性描述符

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

#### 3.2.2 存取属性描述符

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

这其实是等价的，只不过上面的写法能够更该属性描述符，下面的写法比较简洁

这里补充一个小的知识点

!> 在 JavaScript 中一般定义私有属性会在属性的前面加上`_`，这并不是严格意义上的私有，只是在 JS 社区里的一种`规范`，当 JS 程序员看到属性前面有个`_`,就会把他视为是一个私有属性，所以在上面的案例中，我们是可以获取私有属性的

### 3.3 其它方法补充

获取对象的属性描述符：

- getOwnPropertyDescriptor
- getOwnPropertyDescriptors

禁止对象继续添加新的属性

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

禁止对象配置/删除里面的属性

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

让属性不可以修改

详情：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze

```js
var obj = {
  name: "tao",
  age: 18,
};

Object.freeze(obj);

obj.name = "xyj";
console.log(obj.name);
```

## 四、创建多个对象的方案

如果我们现在希望创建一系列的对象：比如 Person 对象

- 包括张三、李四、王五、李雷等等，他们的信息各不相同；
- 那么采用什么方式来创建比较好呢？

目前我们已经学习了两种方式：

- new Object 方式；
- 字面量创建的方式；

这种方式有一个很大的弊端：创建同样的对象时，需要编写重复的代码；

### 4.1 工厂模式

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
var p2 = person("xyj", 21);
```

### 4.2 构造函数

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

**new 操作符调用的作用**

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
var p2 = new Person("xyj", 21);
console.log(p1, p2);
```

![image.png](https://img13.360buyimg.com/ddimg/jfs/t1/108850/10/17839/4012/6146fc97E03155467/689ea3baac88fe90.png)

这个构造函数可以确保我们的对象是有 Person 的类型的（实际是 constructor 的属性，这个我们后续再探讨）；

但是构造函数就没有缺点了吗？

构造函数也是有缺点的，它在于我们需要为每个对象的函数去创建一个函数对象实例；

## 五、原型

### 5.1 对象的原型

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

### 5.2 函数的原型

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
// p1.__proto__.name = 'xyj'
// Foo.prototype.name = 'zm'
// p2.__proto__.name = 'ymy'

console.log(p1.name);
```

通过上面四种方式都可以打印 p1.name

本质上我们修改 p1，p2 的原型，实际上最终修改的是 Foo 构造函数对象的原型，因为 p1，p2 指向的是构造函数对象的原型

### 5.3 constructor 属性

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

### 5.4 重写原型对象

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

### 5.5 原型对象的 constructor

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

### 5.6 构造函数和原型组合

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
var p2 = new Person("xyj", 21);
console.log(p1);
```

这样对吗，不对！很明显这里打印 p1.name 的值是 xyj

因为前面说过，当我们通过.的方式获取值的时候，会触发对应的 get 方法，自己的对象中找不到，就会去原型上找

在 p2 定义的时候把原型对象上的 name 修改为了 xyj，所以 p1 去原型上找的 name 就是 xyj

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
var p2 = new Person("xyj", 21);
```

有人就会好奇在外面定义 say 函数会不会出错啊，这就是考验到 this 的问题了，前面我们说了 this 是动态绑定的，this 的绑定和编写的位置是没有关系的，是跟调用位置有关

这也用到了我们前面所讲的内容，所以我们都是循序渐进的

## 六、原型链

### 6.1 JavaScript 中的类和对象

当我们编写如下代码的时候，我们会如何来称呼这个 Person 呢？

- 在 JS 中严格来说应该称为一个构造函数（因为早期 JS 并没有类的概念）
- 但是如果你从面向对象的编程范式角度来看，Person 应该称之为一个类

```js
function Person() {}

var p1 = new Person();
var p2 = new Person();
```

### 6.2 面向对象的特征-继承

面向对象有三大特性：封装、继承、多态

- 封装：我们前面将属性和方法封装到一个类中，可以称之为封装的过程
- 继承：继承是面向对象中非常重要的，不仅仅可以减少重复代码的数量，也是多态的前提（纯面向对象的前提）
- 多态：不同的对象在执行时表现出不同的形态

我们会核心讲解继承的概念

不着急，我们先来看一下 JavaScript 原型链的机制

然后再通过原型链的机制来实现继承

### 6.3 JavaScript 原型链

前面说过，当我们想获取一个对象的属性的时候，会触发属性内部的 get 方法，会首先在自己的对象中查找属性

如果没有找到会去属性的原型对象中查找

但是其实查找属性也会有跟作用域链类似的原型链的规则

如果自己的对象中没有找到，就会去自己对象的原型对象中查找，如果原型对象中也没有找到，就会去原型对象上的原型对象查找，一直到顶层对象，直到顶层对象的原型对象上也没有则会返回 undefined

![image.png](https://img13.360buyimg.com/ddimg/jfs/t1/111417/39/19533/56220/614c8bf1Eec68cafc/5684df21f1f0ceb4.png)

### 6.4 Object 的原型

那到底顶层对象是什么呢？

其实字面量对象的原型是[Object: null prototype] {}

[Object: null prototype] {}就是顶层原型

那么顶层原型来自哪里呢？

顶层原型其实就是 Object.prototype

我们来看一下，为什么是 Object.prototype

```js
var obj = new Object();
/**
 * var moni = {}
 * this = {}
 * obj.__proto__ = Object.prototype
 * return moni
 */

/**
 * 前面说了new操作父调用的作用（我们再来复习一下）
 * 1. 在内存中创建一个新的对象
 * 2. 构造函数的prototype属性会赋值给对象的[[prototype]]
 * 3. 构造函数的this会指向创建出来的新对象
 * 4. 执行代码
 * 5. 返回新对象
 */

console.log(obj.__proto__); // [Object: null prototype] {}
console.log(Object.prototype); // [Object: null prototype] {}
console.log(obj.__proto__ === Object.prototype); // true
```

这个 Object 的原型对象看上去是空的{}，其实它上面有很多东西

我们可以通过前面讲的 Object.getOwnPropertyDescriptors 方法来获取所有的属性描述符

![image.png](https://img12.360buyimg.com/ddimg/jfs/t1/208596/15/1872/60203/614d302cEc4ae84d2/5eff59ddf48fbf99.png)

那下面我们来看一个例子，然后再通过画图直观的分析一下

```js
var obj = {
  name: "tao",
  age: 18,
};
var obj2 = {};
obj.__proto__ = obj2;

Object.prototype.height = 1.88;

console.log(obj.height);
```

![image.png](https://img14.360buyimg.com/ddimg/jfs/t1/207221/6/2015/57009/614d3690E765437ac/9e1177d56b1572f4.png)

这个图画的有点丑，我简单讲解一下当我们创建一个字面量对象的时候，其实跟 new Object 是差不多的，只不过通过字面量创建并不会实际调用 Object 构造函数，但是从内存的角度中还是会创建 Object 的（这个地方感觉讲的有点瑕疵，后续会查阅资料补充）

obj 的原型对象本来应该是 Object 的原型对象，但是我们手动改变为了原型的指向，obj 的原型对象就变成了 obj2 对象

obj2 对象的原型自然是 Object 的原型对象

所以自然在内存中是上面这幅图

我们想要查看 obj.height 的时候，会首先在自己对象中查找 height 属性，自己的对象中没有 height 属性，就会去原型对象上查找，原型对象 obj2 对象也没有 height 属性，就会去 obj2 对象的原型对象上找，obj2 的原型对象上有 height 属性，就会直接返回，所以最后的结果是 1.88

其实为什么说打印 Object.prototype 为[Object: null prototype] {}

这个[Object: null prototype] {}到底是什么意思呢？

这里我理解为 Object.prototype 上也有原型属性，但是 Object.prototype 的原型属性已经指向的 null 了，也就是说 Object.prototype 就是顶层对象

### 6.5 构造函数的原型

构造函数的顶层原型是 Object 的原型对象

```js
function Person() {}
console.log(Person.prototype); // {}
console.log(Object.getOwnPropertyDescriptors(Person.prototype));
/**
 * constructor: {
    value: [Function: Person],
    writable: true,
    enumerable: false,
    configurable: true
  }
 */
console.log(Person.prototype.__proto__); // [Object: null prototype] {}
```

![image.png](https://img13.360buyimg.com/ddimg/jfs/t1/146769/18/25290/56102/614d41fcE57a7944f/593772343d81ab98.png)

### 6.6 Object 是所有类的父类

通过上面的 Object 和构造函数原型我们可以得出一个结论：原型链的最顶层的原型对象就是 Object 的原型对象

也就是说 Object 是所有类的父类，所有子类继承 Object（后面讲解继承的知识）

## 七、继承

### 7.1 为什么需要继承

总结说来，继承让代码实现共享，提高代码的重用性，子类可以形似父类，也可异于父类，让代码的可扩展性提高，框架中的扩展接口都是通过继承父类实现的，产品或者项目的开发性通过继承得到提高。

### 7.2 原型链的继承方案

```js
function Person() {
  this.name = "tao";
}

Person.prototype.eating = function () {
  console.log(this.name + "eating~");
};
function Student() {
  this.son = 111;
}
Student.prototype = new Person();

Student.prototype.studying = function () {
  console.log(this.name + "studying~");
};

var stu = new Student();
console.log(stu.name);
stu.eating();
```

![image.png](https://img12.360buyimg.com/ddimg/jfs/t1/198706/21/9841/65214/614d88e3E8d967762/0ede08fd14cac64e.png)

但是这种方式还是存在一定的弊端的

这里我简单列举几个问题：

- 打印 stu 对象的时候，继承上的对象是看不到的(JS 的打印机制是不会打印原型对象上的属性的，只会打印当前对象上的属性)
- 创建出来两个 stu 对象进行某些操作是不符合逻辑的

```js
// 首先先在Person上添加this.friends = []
function Person() {
  this.friends = [];
}
var stu1 = new Student();
var stu2 = new Student();
stu1.friends.push("xyj");
console.log(stu1.friends); // ['xyj']
console.log(stu2.friends); // ['xyj']
```

你会发现我给 stu1 添加了一个朋友，但是 stu2 也添加了一个朋友，这明显不符合逻辑

- 不能给Person传递参数，因为这个对象是一次性创建的（没办法定制化）；不能给Person传递参数，因为这个对象是一次性创建的（没办法定制化）；

### 7.3 借用构造函数继承

为了解决原型链继承中存在的问题，社区中提供了一种新的方法：constructor stealing（借用构造函数/经典继承/伪造对象）

steal 是偷窃、剽窃的意思，但是这里可以翻译为借用

借用继承的做法非常简单：在子类型构造函数的内部调用父类型构造函数

- 因为函数可以在任意的时刻被调用
- 因此通过 apply 和 call 方法也可以在新创建的对象上执行构造函数

```js
function Person(name, age, friends) {
  this.name = name;
  this.age = age;
  this.friends = friends;
}

Person.prototype.eating = function () {
  console.log(this.name + "eating~");
};
function Student(name, age, friends) {
  Person.call(this, name, age, friends);
  this.son = 111;
}
Student.prototype = new Person();

Student.prototype.studying = function () {
  console.log(this.name + "studying~");
};

var stu1 = new Student("tao", 18, ["xyj"]);
var stu2 = new Student("xyj", 21, ["tao"]);
stu1.friends.push("zm");
console.log(stu1.friends);
console.log(stu2.friends);
```

但是借用继承也存在一定的弊端：

- Person 对象至少会被调用两次
- stu 的原型对象上会多出一些属性，但是这些属性没有被用到，没有存在的必要

### 7.4 原型式继承函数

原型式继承的渊源

- 这种模式要从道格拉斯·克罗克福德（Douglas Crockford，著名的前端大师，JSON 的创立者）在 2006 年写的
  一篇文章说起: Prototypal Inheritance in JavaScript(在 JS 中使用原型式继承)
- 在这篇文章中，它介绍了一种继承方法，而且这种继承方法不是通过构造函数来实现的.
- 为了理解这种方式，我们先再次回顾一下 JavaScript 想实现继承的目的：重复利用另外一个对象的属性和方法.

这里有三个方法

通过原型实现

```js
var obj = {
  name: "tao",
  age: 18,
};

function create(obj) {
  function fn() {}
  fn.prototype = obj;
  return new fn();
}

var newObj = create(obj);
console.log(newObj.__proto__ === obj); // true
```

通过 Object.setPrototypeOf 实现

```js
var obj = {
  name: "tao",
  age: 18,
};

function create(obj) {
  var newObj = {};
  Object.setPrototypeOf(newObj, obj);
  return newObj;
}

var newObj = create(obj);
console.log(newObj.__proto__ === obj); // true
```

通过 Object.create 实现

```js
var obj = {
  name: "tao",
  age: 18,
};

var newObj = Object.create(obj);
console.log(newObj.__proto__ === obj); // true
```

当然这并不能满足我们的需求，如果我们有很多都想实现继承就会写很多重复的代码，我们其实想要实现的是构造函数之间的继承

### 7.5 寄生式继承函数

寄生式(Parasitic)继承是与原型式继承紧密相关的一种思想, 并且同样由道格拉斯·克罗克福德(Douglas
Crockford)提出和推广的；

寄生式继承的思路是结合原型类继承和工厂模式的一种方式；

即创建一个封装继承过程的函数, 该函数在内部以某种方式来增强对象，最后再将这个对象返回；

```js
var obj = {
  name: "tao",
  age: 18,
};

function create(name) {
  var newObj = Object.create(obj);
  newObj.name = name;
  newObj.say = function () {
    console.log(this.name + "say~");
  };
  return newObj;
}

var newObj1 = create("tao");
var newObj2 = create("xyj");
var newObj3 = create("zm");
console.log(newObj1.__proto__ === obj); // true
```

虽然这种方式也可以实现，但是也相对于是前面讲到的工厂模式差不多，相对于每一个实现的对象都有自己的方法，也没有属于自己的类型

### 7.6 寄生组合式继承

现在我们来回顾一下之前提出的比较理想的组合继承

- 组合继承是比较理想的继承方式, 但是存在两个问题:
- 问题一: 构造函数会被调用两次: 一次在创建子类型原型对象的时候, 一次在创建子类型实例的时候.
- 问题二: 父类型中的属性会有两份: 一份在原型对象中, 一份在子类型实例中.

事实上, 我们现在可以利用寄生式继承将这两个问题给解决掉.

- 你需要先明确一点: 当我们在子类型的构造函数中调用父类型.call(this, 参数)这个函数的时候, 就会将父类型中
  的属性和方法复制一份到了子类型中. 所以父类型本身里面的内容, 我们不再需要.
- 这个时候, 我们还需要获取到一份父类型的原型对象中的属性和方法

能不能直接让子类型的原型对象 = 父类型的原型对象呢?

- 不要这么做, 因为这么做意味着以后修改了子类型原型对象的某个引用类型的时候, 父类型原生对象的引用类型
  也会被修改.
- 我们使用前面的寄生式思想就可以了

```js
function Person(name, age, friends) {
  this.name = name;
  this.age = age;
  this.friends = friends;
}

Person.prototype.say = function () {
  console.log(this.name + "say~");
};

function Student(name, age, friends) {
  Person.call(this, name, age, friends);
}

Student.prototype = Object.create(Person.prototype);

Student.prototype.eating = function () {
  console.log(this.name + "eating~");
};

var stu = new Student("tao", 18, "xyj");
console.log(stu);
stu.eating();
stu.say();
```

当然这也存在一定的弊端，这里打印的 stu 的类型是一个 Person 这肯定是不符合我们的预期的

这是为什么喃？

让我们通过内存图来看一下整个过程

![image.png](https://img12.360buyimg.com/ddimg/jfs/t1/198158/39/10142/56282/614f37e3Efd903618/5ca1b0a60d2f38cd.png)

打印一个对象，打印出来的结果前面的类型其实是打印原型对象上的 constructor 属性

Student 原型对象是通过 Object.create 方法返回的新的对象，返回的新的对象是没有 constructor 属性，没有它就会去返回的新的对象的原型上找 constructor 属性，Perosn 的 constructor 指向的是 Person，所以打印出来的 stu 的类型是 Person

我们前面讲过可以通过属性描述符来精准添加控制 constructor 属性

```js
Object.defineProperty(Student.prototype, "constructor", {
  configurable: true,
  writable: true,
  enumerable: false,
  value: Student,
});
```

这样就达到了我们的预期

但是我们如果是把这些东西写死的话就太死板了，所以我们还要再进行优化一下，一般我们会封装成一个函数

```js
function create(obj, newObj) {
  newObj.prototype = Object.create(obj);
  Object.defineProperty(newObj.prototype, "constructor", {
    configurable: true,
    enumerable: false,
    writable: true,
    value: newObj,
  });
}
```

Object.create 算是比较新的语法，如果要考虑兼容的话，一般我们会写成这样

```js
function object(obj) {
  function fn() {}
  fn.prototype = obj;
  return new fn();
}

function create(obj, newObj) {
  newObj.prototype = object(obj.prototype);
  Object.defineProperty(newObj.prototype, "constructor", {
    configurable: true,
    enumerable: false,
    writable: true,
    value: newObj,
  });
}
create(Person, Student);
```

这就实现了我们想要的效果

看看最终代码

```js
function Person(name, age, friends) {
  this.name = name;
  this.age = age;
  this.friends = friends;
}

Person.prototype.say = function () {
  console.log(this.name + "say~");
};

function Student(name, age, friends) {
  Person.call(this, name, age, friends);
}

function object(obj) {
  function fn() {}
  fn.prototype = obj;
  return new fn();
}

function create(obj, newObj) {
  newObj.prototype = object(obj.prototype);
  Object.defineProperty(newObj.prototype, "constructor", {
    configurable: true,
    enumerable: false,
    writable: true,
    value: newObj,
  });
}
create(Person, Student);

Student.prototype.eating = function () {
  console.log(this.name + "eating~");
};

var stu = new Student("tao", 18, "xyj");
var p = new Person();
console.log(p);
console.log(stu);
stu.eating();
stu.say();
```

## 八、其它补充

### 8.1 hasOwnProperty

详情：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwnProperty

判断一个对象自身是否有指定的属性，返回布尔值

```js
var obj = {
  name: "tao",
  age: 18,
};

obj.__proto__ = {
  height: 1.88,
};

console.log(obj.hasOwnProperty("age")); // true
console.log(obj.hasOwnProperty("height")); // false
```

### 8.2 in 操作符

详情：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/in

判断指定的属性是否在指定的对象或其原型链中，返回布尔值

```js
var obj = {
  name: "tao",
  age: 18,
};

obj.__proto__ = {
  height: 1.88,
};

console.log("age" in obj); // true
console.log("height" in obj); // true
```

这就可以延伸出 for...in 操作符

遍历对象的属性，仅遍历自身的属性

详情：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...in

```js
var obj = {
  name: "tao",
  age: 18,
};

obj.__proto__ = {
  height: 1.88,
};

for (const key in obj) {
  console.log(key); // name,age,height
}
```

### 8.3 instanceof

详情：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/instanceof

用于检测`构造函数的 prototype` 属性是否出现在`某个实例对象的原型链`上,返回一个布尔值

```js
function Person(name, age) {
  this.name = name;
  this.age = age;
}

function Student(name, age) {
  Person.call(name, age);
}

var stu = new Student("tao", 18);
console.log(stu instanceof Person); // false
console.log(stu instanceof Object); // true
```

![image.png](https://img14.360buyimg.com/ddimg/jfs/t1/199167/16/10015/82860/614fd4c0E32ebde09/d130f9074462c10b.png)

我简单画了一下内存图，从内存图来看就可以直观看出 instanceof 的用法了

### 8.4 isPrototypeOf

详情：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/isPrototypeOf

用于检测某个对象，是否出现在某个实例对象的原型链上,返回一个布尔值

```js
function Person(name, age) {
  this.name = name;
  this.age = age;
}

function Student(name, age) {
  this.name = name;
  this.age = age;
}

var stu = new Student("tao", 18);
var p = new Person("xyj", 21);

console.log(p.__proto__.isPrototypeOf(stu)); // false
console.log(Object.prototype.isPrototypeOf(stu)); // true
```

## 九、对象、函数、原型之间的关系

千言万语汇成一张图

![](http://www.mollypages.org/tutorials/jsobj.jpg)

我下面简单说说我对这张图的看法

我先画一张内存图，然后再来进行讲解

![image.png](https://img11.360buyimg.com/ddimg/jfs/t1/202129/30/8265/111037/614fe0c8E61c8127d/e53fc2518d7824de.png)

我感叹我的画图水平，应该只有我自己看的懂吧

我还是简单说一下吧（巴拉巴拉）

每一个对象都有一个隐式原型对象(**proto**)

每一个函数都有一个显式原型对象(prototype),因为函数又是一个对象，所以函数也有一个隐式原型对象(**proto**)

假如我创建了一个 Person 构造函数，通过 new 关键字创建了一个 p 对象，p 对象的**proto**当然指向的是 Person 构造函数的原型对象（new 操作符的作用，前面讲过）

Person 构造函数的**proto**当然指向的是 Person 的原型对象，Person 构造函数的 prototype 指向的是 Function 的原型对象，Person 的原型对象上的**proto**当然指向的是 Object 的原型对象（这前面说过了）

那么这个 Function 是怎么来的？

其实全局下有一个 Function 的构造函数

我们通过直接 function 定义函数，其实是跟 new Function 是相似的（但其实还有很不同，后续看看书上的讲解，再来补充），所以 Person 构造函数的 prototype 指向的是 Function 的原型对象

接下来就要说说 Function 这个构造函数了，Function 的**proto**当然指向的是 Function 的原型对象，Function 的 prototype 其实也是指向的是 Function 的原型对象，这有点特别

我感觉我说的有点怪，具体还是看上面的 tutorials 图为准

Function 的原型对象的**proto**的指向当然也是 Object 的原型对象

下面就要说 Object 构造函数了，这个构造函数又是怎么来的？

其实 Object 构造函数是通过 new Function 创建出来的

所以 Object 的 prototype 指向的就是 Function 的原型对象，Object 构造的**proto**当然指向的就是 Object 原型对象

Object 原型对象的**proto**指向的是 null，所以 Object 是顶层对象

这就是整个的逻辑，当然我讲的不是很清晰，具体还要根据前面讲的知识然后结合哪张图，就会真正理解原型的继承关系

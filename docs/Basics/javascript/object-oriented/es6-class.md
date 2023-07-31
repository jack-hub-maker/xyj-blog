## 一、class 定义类的方式

我们会发现，按照前面的构造函数形式创建 类，不仅仅和编写普通的函数过于相似，而且代码并不容易理解。

- 在 ES6（ECMAScript2015）新的标准中使用了 class 关键字来直接定义类；
- 但是类`本质`上依然是前面所讲的构造函数、原型链的`语法糖`而已；
- 所以学好了前面的构造函数、原型链更有利于我们理解类的概念和继承关系；

那么，如何使用 class 来定义一个类呢？

可以使用两种方式来声明类：

- 类声明
- 类表达式

### 1.1 类声明

```js
class Person {}
```

### 1.2 类表达式

```js
// 跟函数表达式差不多，但是类使用表达式这种方式用的不多
var info = class {};
```

我们来研究一下类的一些特性：

你会发现它和我们的构造函数的特性其实是一致的；

```js
var p = new Person();
console.log(typeof Person); // function
console.log(p.__proto__ == Person.prototype); // true
console.log(Person.prototype.__proto__ === Object.prototype); // true
```

## 二、class 的构造方法

如果我们希望在创建对象的时候给类传递一些参数，这个时候应该如何做呢？

- 每个类都可以有一个自己的构造函数（方法），这个方法的名称是固定的 **constructor**;
- 当我们通过 new 操作符，操作一个类的时候会调用这个类的构造函数 constructor；
- 每个类只能有`一个构造函数`，如果包含多个构造函数，那么会抛出异常；

当我们通过 new 关键字操作类的时候，会调用这个 constructor 函数，并且执行如下操作:

1.  在内存中创建一个新的对象（空对象）；
2.  这个对象内部的 proto 属性会被赋值为该类的 prototype 属性；
3.  构造函数内部的 this，会指向创建出来的新对象；
4.  执行构造函数的内部代码（函数体代码）；
5.  如果构造函数没有返回非空对象，则返回创建出来的新对象；

```js
class Perosn {
  // 构造方法
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
}
```

## 三、class 中的方法定义

### 3.1 class 的实例方法

在上面我们定义的属性都是直接放到了 this 上，也就意味着它是放到了创建出来的新对象中：

- 在前面我们说过对于实例的方法，我们是希望放到原型上的，这样可以被多个实例来共享；
- 这个时候我们可以直接在类中定义；

```js
class Perosn {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  // 实例方法
  say() {
    console.log(this.name + " say~");
  }
  eating() {
    console.log(this.name + " eating~");
  }
}
```

### 3.2 class 的访问器方法

我们之前讲对象的属性描述符时有讲过对象可以添加 setter 和 getter 函数的，那么类也是可以的：

```js
class Perosn {
  constructor(name) {
    this._name = name;
  }
  get name() {
    console.log("拦截到访问方法");
    return this._name;
  }
  set name(newValue) {
    console.log("拦截到设置方法");
    this._name = newValue;
  }
}

var p = new Perosn("tao");
p.name = "xyj";
console.log(p.name);
```

### 3.3 class 的静态方法

静态方法通常用于定义直接使用类来执行的方法，不需要有类的实例，使用 static 关键字来定义：

看下面的案例：写一个批量生产 person 的静态方法

```js
var names = ["tao", "xyj", "zm"];

class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  static createPerson() {
    var nameIndex = Math.floor(Math.random() * names.length);
    var name = names[nameIndex];
    var age = Math.floor(Math.random() * 100);
    return new Person(name, age);
  }
}

for (var i = 0; i < 10; i++) {
  console.log(Person.createPerson());
}
```

## 四、class 中实现继承 extends

前面我们花了很大的篇幅讨论了在 ES5 中实现继承的方案，虽然最终实现了相对满意的继承机制，但是过程却依然
是非常繁琐的。

在 ES6 中新增了使用 extends 关键字，可以方便的帮助我们实现继承：

```js
class Person {}
// 表示Student继承Person
class Student extends Peron {}
```

### 4.1 super 关键字

我们会发现在上面的代码中我使用了一个 super 关键字，这个 super 关键字有不同的使用方式：

注意：在子（派生）类的构造函数中使用 this 或者返回默认对象之前，必须先通过 super 调用父类的构造函数！

```js
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
}
class Student extends Person {
  constructor(name, age, friends) {
    // 错误的做法
    // this.name = name
    // this.age = age

    // 正确的做法
    super(name, age);
    this.friends = friends;
  }
}

var s = new Student("tao", 18, ["zs", "ls"]);
```

super 的使用位置有三个：

- 子类的构造方法
- 实例方法
- 静态方法

```js
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  say() {
    console.log(this.name + " Person say~");
  }
  static process() {
    console.log("处理逻辑1");
    console.log("处理逻辑2");
    console.log("处理逻辑3");
  }
}
class Student extends Person {
  constructor(name, age, friends) {
    super(name, age);
    this.friends = friends;
  }
  // 如果父类有同样的实例方法，我们在子类中再写同样的方法（我们称之为重写父类的方法）
  // 如果我们想调用了父类的方法之后再调用子类的方法,就可以使用super进行调用
  say() {
    super.say();
    console.log(this.name + " Student say~");
  }
  static process() {
    super.process();
    console.log("处理逻辑4");
    console.log("处理逻辑5");
    console.log("处理逻辑6");
  }
}

var s = new Student("tao", 18, ["zs", "ls"]);
s.say();
Student.process();
```

## 五、继承内置类

我们也可以让我们的类继承自内置类，比如 Array：

```js
class TAOArray extends Array {
  firstItem() {
    return this[0];
  }
  lastItem() {
    return this[this.length - 1];
  }
}

var arr = new TAOArray(1, 2, 3);
console.log(arr.firstItem()); // 1
console.log(arr.lastItem()); // 3
```

!>对于传统的面向对象语言，这种方式用的比较多，但是在 JS 里面，这种方式用的不多（因为 JS 支持多种编程范式，使用函数来进行封装是用的比较多的）

## 六、JS 中实现混入效果

JS 其实是`没有混入`的概念的，只是**通过一些技巧**实现类似混入的效果

JavaScript 的类只支持`单继承`：也就是只能有一个父类

- 那么在开发中我们我们需要在一个类中添加更多相似的功能时，应该如何来做呢？
- 这个时候我们可以使用混入（mixin）；

```js
class Person {}
class Man {}

function mixinMan(BaseClass) {
  return class extends BaseClass {
    constructor(name, age, gender, friends) {
      super(name, age);
      this.gender = gender;
      this.friends = friends;
    }
    say() {
      console.log(this.name + " say~");
    }
  };
}

function mixinPerson(BaseClass) {
  return class extends BaseClass {
    constructor(name, age, gender, friends) {
      super(gender, friends);
      this.name = name;
      this.age = age;
    }
    sports() {
      console.log(this.name + " sports~");
    }
  };
}

class Student extends mixinMan(mixinPerson(Person)) {
  constructor(name, age, gender, friends) {
    super(name, age, gender, friends);
  }
}

var s = new Student("tao", 19, "man", ["zs"]);
console.log(s); // Student { name: 'tao', age: 19, gender: 'man', friends: [ 'zs' ] }
s.sports(); // tao sports~
s.say(); // tao say~
```

## 七、JS 面向对象多态

面向对象的三大特性

- 封装
- 继承
- 多态

前面两个我们都已经详情解析过了，接下来我们讨论一下 JavaScript 的多态

首先我们先来看传统面向对象的多态（这里就以 TS 来进行举例）

### 7.1 传统面向对象多态

传统的面向对象多态是有三个前提的：

1.  必须有继承(多态的前提)
2.  必须有重写(子类重写父类的方法)
3.  必须有父类引用指向子类对象

```ts
class shape {
  area() {}
}

class circle extends shape {
  area() {
    return 100;
  }
}

class square extends shape {
  area() {
    return 200;
  }
}

var c = new circle();
var s = new square();

// 这里的父类引用指向子类对象其实也是有的
// 这里我们要求传入的参数是一个shape类型
// 等价于 var shape:shape =  new circle
// shape是不是就是父类的引用,new circle是不是一个子类的对象
// 所以这里是满足父类引用指向子类对象的
function calcArea(shape: shape) {
  console.log(shape.area());
}

calcArea(c);
calcArea(s);
```

接下来我们再来看 JS 面向对象多态

### 7.2 JS 面向对象多态

JavaScript 有多态吗？

- 首先我们先来看一下维基百科对多态的定义：
  - 在编程语言和类型论中，`多态`（英语：polymorphism）指的是
    - 为不同数据类型的实体提供统一的接口
    - 或使用一个单一的符号来表示多个不同的类型。
  - 简单来说：**不同的数据类型进行同一个操作，表现出不同的行为，就是多态的体现**。

那么从上面的定义来看，我觉得 JavaScript 是一定存在多态的。

```js
function sum(num1, num2) {
  return num1 + num2;
}
sum(10, 20);
sum("abc", "cba");
```

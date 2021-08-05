---
title: 类
---

## 前言

在早期的 JavaScript 开发中（ES5）我们需要通过函数和原型链来实现类和继承，从 ES6 开始，引入了 class 关键字，可以
更加方便的定义和使用类。

TypeScript 作为 JavaScript 的超集，也是支持使用 class 关键字的，并且还可以对类的属性和方法等进行静态类型检测。

实际上在 JavaScript 的开发过程中，我们更加习惯于函数式编程：

- 比如 React 开发中，目前更多使用的函数组件以及结合 Hook 的开发模式；
- 比如在 Vue3 开发中，目前也更加推崇使用 Composition API；

但是在封装某些业务的时候，类具有更强大封装性，所以我们也需要掌握它们。

类的定义我们通常会使用 class 关键字：

- 在面向对象的世界里，任何事物都可以使用类的结构来描述；
- 类中包含特有的属性和方法；

## 类的定义

我们来定义一个 Person 类：

使用 class 关键字来定义一个类；

我们可以声明一些类的属性：在类的内部声明类的属性以及对应的类型

- 如果类型没有声明，那么它们默认是 any 的；
- 我们也可以给属性设置初始化值；
- 在默认的 strictPropertyInitialization 模式下面我们的属性是必须初始
  化的，如果没有初始化，那么编译时就会报错；

类可以有自己的构造函数 constructor，当我们通过 new 关键字创建一个
实例时，构造函数会被调用；

- 构造函数不需要返回任何值，默认返回当前创建出来的实例；

类中可以有自己的函数，定义的函数称之为方法；

```ts
class Person {
  name: string;
  age: number;
  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }
  say(): void {
    console.log("Hello World");
  }
}
const p = new Person("tao", 19);
console.log(p.name); // tao
console.log(p.age); // 19
p.say(); // Hello World
```

## 类的继承

面向对象的其中一大特性就是继承，继承不仅仅可以减少我们的代码量，也是多态的使用前提。

我们使用 extends 关键字来实现继承，子类中使用 super 来访问父类。

我们来看一下 Student 类继承自 Person：

- Student 类可以有自己的属性和方法，并且会继承 Person 的属性和方法；
- 在构造函数中，我们可以通过 super 来调用父类的构造方法，对父类中的属性进行初始化；

```ts
class Person {
  name: string;
  age: number;
  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }
  say(): void {
    console.log("Hello Person");
  }
}

class Student extends Person {
  height: number;
  constructor(name: string, age: number, height: number) {
    // super调用父类的构造器
    super(name, age);
    this.height = height;
  }
  /**
   * 方法也会继承
   * 所以在子类可以继承父类的方法
   * 子类也可以重写父类的方法
   * 继承过来的方法,在子类中也出现出现了,那么会覆盖(重写)
   */
  say(): void {
    // 但是有些情况我重写了方法,但是我又想调用一次父类继承过来的方法
    // 通过super.xx可以调用父类继承过来的方法
    super.say();
    console.log("Hello Student");
  }
}

const s = new Student("tao", 19, 1.85);
s.say(); // Hello Person Hello Student
```

## 类的多态

关于类的多态属于面向对象的基础,这里我就简单说一下

想要实现多态的前提就是**父类引用指向子类对象**

如何理解?我们来看一个案例

```ts
class Animal {
  action() {
    console.log("animal running");
  }
}
class Dog extends Animal {
  action() {
    console.log("dog running");
  }
}

class Fish extends Animal {
  action() {
    console.log("fish running");
  }
}

function mackActions(animals: Animal[]) {
  // 请问这里遍历出来的item.action执行那个的action
  animals.forEach((item) => {
    item.action();
  });
}

// 父类的Animal引用了子类对象
// const animal1 = new Dog()
// const animal2 = new Fish()
mackActions([new Dog(), new Fish()]);

// 当然我们可以使用其它方法也是可以做出这样的效果的(比如前面学过的函数重载,联合类型)
// 多态的目的是为了写出更加具备通用性的代码
```

## 类的成员修饰符

在 TypeScript 中，类的属性和方法支持三种修饰符： public、private、protected

- public 修饰的是在任何地方可见、公有的属性或方法，默认编写的属性就是 public 的；
- private 修饰的是仅在同一类中可见、私有的属性或方法；
- protected 修饰的是仅在类自身及子类中可见、受保护的属性或方法；

public 是默认的修饰符，也是可以直接访问的，我们这里来演示一下 protected 和 private。

```ts
class Person {
  private name: string = "tao";
  say() {
    console.log(this.name);
  }
}

const p = new Person();
// 报错 属性'name'是私有的，只能在'Person'类中访问。
console.log(p.name);
// 直接访问的话是不行的，但是我们可以通过类中的其它方法来进行访问
p.say(); // tao
```

```ts
class Person {
  protected name: string = "tao";
  say() {
    console.log(this.name);
  }
}

class Student extends Person {
  getPerson() {
    console.log(this.name);
  }
}

const s = new Student();
s.getPerson(); // tao
```

## 只读属性 readonly

如果有一个属性我们不希望外界可以任意的修改，只希望确定值后直接使用，那么可以使用 readonly：

```ts
class Person {
  readonly name: string;
  // 只读属性可以在构造器中赋值,赋值之后就不可以修改
  // 属性本身是无法修改的,但是如果它是对象类型,对象中的属性是可以修改的
  constructor(name: string) {
    this.name = name;
  }
  say() {
    console.log(this.name);
  }
}

class Student extends Person {
  getPerson() {
    console.log(this.name);
  }
}

const p = new Person("tao");
p.name = "31"; // 不能赋值给'name'，因为它是一个只读的属性。
```

## getters/setters

在前面一些私有属性我们是不能直接访问的，或者某些属性我们想要监听它的获取(getter)和设置(setter)的过程，
这个时候我们可以使用存取器。

```ts
class Person {
  private _name: string;
  constructor(name: string) {
    this._name = name;
  }
  // 访问器getter/setter
  // getter
  get name() {
    return this._name;
  }
  // setter
  set name(newName) {
    this._name = newName;
  }
}

const p = new Person("tao");
p.name = "sandy";
console.log(p.name); // sandy
```

## 静态成员

前面我们在类中定义的成员和方法都属于对象级别的, 在开发中, 我们有时候也需要定义类级别的成员和方法。

在 TypeScript 中通过关键字 static 来定义：

```ts
// 如果我们想获取Person的name一般都是要通过new一个实例对象,然后才可以获取
// 但是我们也可以通过设置静态属性的方法来直接获取
class Perosn {
  static age: number = 19;
  static say() {
    console.log("Hello Person");
  }
}
console.log(Perosn.age); // 19
Perosn.say(); // Hello Person
```

## 抽象类 abstract

我们知道，继承是多态使用的前提。

- 所以在定义很多通用的调用接口时, 我们通常会让调用者传入父类，通过多态来实现更加灵活的调用方式。
- 但是，父类本身可能并不需要对某些方法进行具体的实现，所以父类中定义的方法,，我们可以定义为抽象方法。

什么是 抽象方法? 在 TypeScript 中没有具体实现的方法(没有方法体)，就是抽象方法。

- 抽象方法，必须存在于抽象类中；
- 抽象类是使用 abstract 声明的类；

抽象类有如下的特点：

- 抽象类是不能被实例的话（也就是不能通过 new 创建）
- 抽象方法必须被子类实现，否则该类必须是一个抽象类；

```ts
function calculationArea(shape: Shape) {
  return shape.getarea();
}

abstract class Shape {
  abstract getarea();
}

class Rectangular extends Shape {
  width: number;
  height: number;
  constructor(width: number, height: number) {
    super();
    this.width = width;
    this.height = height;
  }
  getarea() {
    return this.height * this.width;
  }
}

class Round extends Shape {
  radius: number;
  constructor(radius: number) {
    super();
    this.radius = radius;
  }
  getarea() {
    return this.radius * this.radius * 3.14;
  }
}

const rectangular = new Rectangular(10, 20);
const round = new Round(10);

console.log(calculationArea(rectangular));
console.log(calculationArea(round));

calculationArea(123); // 报错
calculationArea("12312"); // 报错
```

## 类的类型

类本身也是可以作为一种数据类型的：

```ts
class Person {
  name: string = "tao";
  say() {
    console.log("Hello Person");
  }
}

const p = new Person();
// 设置了p2的类型为Person
// 那么p2必须实现Person中的属性和方法
const p2: Person = {
  name: "sandy",
  say() {},
};

// 应用场景

/**
 * 一个函数,参数是一个类,获取类中的name属性
 * 添加了类型注解,我们不仅可以实例一个类,还可以传一个对象,对象实现了类的属性和方法即可
 */

function printPerson(p: Person) {
  console.log(p.name);
}

printPerson(new Person());
printPerson({ name: "sandy", say() {} });
```

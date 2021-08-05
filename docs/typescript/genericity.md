---
title: 泛型
---

## 认识泛型

软件工程的主要目的是构建不仅仅明确和一致的 API，还要让你的代码具有很强的可重用性：

- 比如我们可以通过函数来封装一些 API，通过传入不同的函数参数，让函数帮助我们完成不同的操作；
- 但是对于参数的类型是否也可以参数化呢？

什么是类型的参数化？

- 我们来提一个需求：封装一个函数，传入一个参数，并且返回这个参数；

如果我们是 TypeScript 的思维方式，要考虑这个参数和返回值的类型需要一致：

```ts
// 在定义函数的时候,我不告诉这些参数的类型
// 而是让我在调用的时候.告诉我的参数类型应该是什么类型
function foo(bar: number): number {
  return bar;
}
```

上面的代码虽然实现了，但是不适用于其他类型，比如 string、boolean、Person 等类型：

```ts
function foo(bar: any): any {
  return bar;
}
```

## 泛型实现类型参数化

虽然 any 是可以的，但是定义为 any 的时候，我们其实已经丢失了类型信息：

- 比如我们传入的是一个 number，那么我们希望返回的可不是 any 类型，而是 number 类型；
- 所以，我们需要在函数中可以捕获到参数的类型是 number，并且同时使用它来作为返回值的类型；

我们需要在这里使用一种特性的变量 - **类型变量**（type variable），它作用于类型，而不是值：

```ts
function foo<T>(arg: T): T {
  return arg;
}

foo<number>(3);
```

这里我们可以使用两种方式来调用它：

- 通过 <类型> 的方式将类型传递给函数；
- 通过类型推导，自动推到出我们传入变量的类型：
  - 在这里会推导出它们是 字面量类型的，因为字面量类型对于我们的函数也是适用的

```ts
function foo<T>(arg: T): T {
  return arg;
}

foo(3);
foo("123");
```

## 泛型的基本补充

当然我们也可以传入多个类型：

```ts
function foo<T, K, V>(arg1: T, arg2: K, arg3: V) {}

foo<number, number, string>(1, 4, "tao");
```

平时在开发中我们可能会看到一些常用的名称：

- T：Type 的缩写，类型
- K、V：key 和 value 的缩写，键值对
- E：Element 的缩写，元素
- O：Object 的缩写，对象

## 泛型接口

在定义接口的时候我们也可以使用泛型：

```ts
interface IPerson<T> {
  age: T;
  list: T[];
  say: (value: T) => void;
}

const info: IPerson<number> = {
  age: 19,
  list: [1, 2, 3],
  say: (value) => {
    console.log(value);
  },
};
```

也可以在定义接口的时候给泛型接口指定默认值

```ts
interface IPerson<T = number> {
  age: T;
  list: T[];
  say: (value: T) => void;
}
const info: IPerson = {
  age: 19,
  list: [1, 2, 3],
  say: (value) => {
    console.log(value);
  },
};
```

## 泛型类

我们也可以编写一个泛型类：

```ts
class Person<T> {
  x: T;
  y: T;
  constructor(x: T, y: T) {
    this.x = x;
    this.y = y;
  }
}

const p1 = new Person<number>(10, 20);
const p2 = new Person(10, 20);
const p3: Person<number> = new Person(10, 20);
```

## 泛型约束

有时候我们希望传入的类型有某些共性，但是这些共性可能不是在同一种类型中：

- 比如 string 和 array 都是有 length 的，或者某些对象也是会有 length 属性的；
- 那么只要是拥有 length 的属性都可以作为我们的参数类型，那么应该如何操作呢？

```ts
interface ILength {
  length: number;
}

function getLength<T extends ILength>(arg: T) {
  return arg.length;
}

getLength("1321");
getLength([131]);
getLength({ length: 132 });
```

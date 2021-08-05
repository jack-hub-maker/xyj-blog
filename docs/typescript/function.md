---
title: 函数
---

## 函数类型

在 JavaScript 开发中，函数是重要的组成部分，并且函数可以作为一等公民（可以作为参数，也可以作为返回值进
行传递）。

那么在使用函数的过程中，函数是否也可以有自己的类型呢？

- 我们可以编写函数类型的表达式（Function Type Expressions），来表示函数类型；

```ts
function sum(num1: number, num2: number): number {
  return num1 + num2;
}

function mul(num1: number, num2: number): number {
  return num1 * num2;
}

type CalcFnType = (num1: number, num2: number) => void;
function calc(fn: CalcFnType) {
  console.log(fn(20, 30));
}

calc(sum); // 50
calc(mul); // 600
```

在上面的语法中 (num1: number, num2: number) => void，代表的就是一个函数类型：

- 接收两个参数的函数：num1 和 num2，并且都是 number 类型；
- 并且这个函数是没有返回值的，所以是 void；

在某些语言中，可能参数名称 num1 和 num2 是可以省略，但是 TypeScript 是不可以的：

## 参数的可选类型

我们可以指定某个参数是可选的：

```ts
function foo(name: string, age?: number) {
  console.log(name, age);
}
```

这个时候这个参数 age 依然是有类型的，它是什么类型呢？ number | undefined,这个在前面已经讲过了

另外可选类型需要在必传参数的后面：

![](/type-script/14.png)

## 默认参数

从 ES6 开始，JavaScript 是支持默认参数的，TypeScript 也是支持默认参数的：

```ts
function foo(name: string, age: number = 19) {
  console.log(name, age);
}

foo("tao");
```

这个时候 age 的类型其实是 undefined 和 number 类型的联合。

:::tip 提示
个人习惯:开发中我会选择先写必传参数->默认值参数->可选参数
:::

## 剩余参数

从 ES6 开始，JavaScript 也支持剩余参数，剩余参数语法允许我们将一个不定数量的参数放到一个数组中。

```ts
function sum(...nums: number[]) {
  nums.forEach((item) => {
    console.log(item);
  });
}

sum(20, 30, 40);
```

## 可推导的 this

this 是 JavaScript 中一个比较难以理解和把握的知识点：

那么，TypeScript 是如何处理 this 呢？我们先来看一个例子：

```ts
const info = {
  name: "tao",
  say() {
    console.log(this.name);
  },
};
info.say();
```

上面的代码是可以正常运行的，也就是 TypeScript 在编译时，认为我们的 this 是可以正确去使用的：

- TypeScript 认为函数 say 有一个对应的 this 的外部对象 info，所以在使用时，就会把 this 当做该对象。

## 不确定的 this

但是对于某些情况来说，我们并不知道 this 到底是什么？

```ts
function say() {
  console.log(this.name);
}

const info = {
  name: "tao",
  say,
};
info.say();
```

这段代码运行会报错的：

- 这里我们再次强调一下，TypeScript 进行类型检测的目的是让我们的代码更加的安全；
- 所以这里对于 say 的调用来说，我们虽然将其放到了 info 中，通过 info 去调用，this 依然是指向 info 对象的；
- 但是对于 TypeScript 编译器来说，这个代码是非常不安全的，因为我们也有可能直接调用函数，或者通过别的对象来
  调用函数；

## 指定 this

这个时候，通常 TypeScript 会要求我们明确的指定 this 的类型：

```ts
type SayThisType = { name: string };
function say(this: SayThisType, age: number) {
  console.log(this.name, age);
}

const info = {
  name: "tao",
  say,
};

// 直接调用是不行的
// say({ name: 'tao' })

// 隐式调用
info.say(19);

// 显式调用
say.call({ name: "sandy" }, 21);
say.apply({ name: "zm" }, [20]);
```

## 函数重载

简单来说：函数重载就是函数的名称相同，但是参数不同的几个函数

在 TypeScript 中，如果我们编写了一个 add 函数，希望可以对字符串和数字类型进行相加，应该如何编写呢？

我们可能会这样来编写，但是其实是错误的：

```ts
function add(a1: number | string, a2: number | string) {
  return a1 + a2;
}
```

![](/type-script/15.png)

那么这个代码应该如何去编写呢？

- 在 TypeScript 中，我们可以去编写不同的重载签名（overload signatures）来表示函数可以以不同的方式进行
  调用；
- 一般是编写两个或者以上的重载签名，再去编写一个通用的函数以及实现；

比如我们对 sum 函数进行重构：

在我们调用 sum 的时候，它会根据我们传入的参数类型来决定执行函数体时，到底执行哪一个函数的**重载签名**；

```ts
// 声明函数
function add(a1: number, a2: number): number;

function add(a1: string, a2: string): string;

// 实现函数
function add(a1: any, a2: any): any {
  if (typeof a1 === "string" && typeof a2 === "string") {
    return a1.length + a2.length;
  }
  return a1 + a2;
}

console.log(add(20, 30)); // 50
console.log(add("abc", "cba")); // 6
```

但是注意，有实现提的函数，是不能直接被调用的：

![](/type-script/16.png)

调用重载的函数,是通过从上往下执行查看函数签名是否符合要求,符合的话则执行下面的实现函数,不符合则报错

### 练习

我们现在有一个需求：定义一个函数，可以传入字符串或者数组，获取它们的长度。

这里有两种实现方案：

- 使用联合类型来实现；
- 实现函数重载来实现；

```ts
function getLength(a: string | any[]): number {
  return a.length;
}

console.log(getLength("abc")); // 3
console.log(getLength([123, 321, 1234567])); // 3
```

```ts
function getLength(a: string): number;
function getLength(a: any[]): number;

function getLength(a: any): any {
  return a.length;
}

console.log(getLength("abc")); // 3
console.log(getLength([123, 321, 1234567])); // 3
```

在开发中我们选择使用哪一种呢？

- 在可能的情况下，尽量选择使用联合类型来实现(简单用联合,复杂用重载)

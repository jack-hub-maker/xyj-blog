---
title: 类型补充
---

## 函数

### 函数的参数和返回值类型

函数是 JavaScript 非常重要的组成部分，TypeScript 允许我们指定函数的参数和返回值的类型。

声明函数时，可以在每个参数后添加类型注解，以声明函数接受的参数类型：

我们也可以添加返回值的类型注解，这个注解出现在函数列表的后面：

```ts
function sum(num1: number, num2: number): number {
  return num1 + num2;
}
```

和变量的类型注解一样，我们通常情况下不需要返回类型注解，因为 TypeScript 会根据 return 返回值推断函数的
返回类型：

- 某些第三方库处于方便理解，会明确指定返回类型，但是这个看**个人喜好**；

### 上下文函数的参数类型

当一个函数出现在 TypeScript 可以确定该函数会被如何调用的地方时；

该函数的参数会自动指定类型；

```ts
const names = ["tao", "sandy", "zm"];
names.forEach((item) => {});
```

![](/type-script/10.png)

我们并没有指定 item 的类型，但是 item 是一个 string 类型：

- 这是因为 TypeScript 会根据 forEach 函数的类型以及数组的类型推断出 item 的类型；'
- 这个过程称之为**上下文类型**（contextual typing），因为函数执行的上下文可以帮助确定参数和返回值的类型；

## 对象类型

如果我们希望限定一个函数接受的参数是一个对象，这个时候要如何限定呢？

- 我们可以使用对象类型；

```ts
function coordinate(coordinate: { x: number; y: number; z: number }) {
  console.log(coordinate.x);
  console.log(coordinate.y);
  console.log(coordinate.z);
}

coordinate({ x: 12, y: 123, z: 213 });
```

## 可选类型

当我们设立形参的时候,在 ts 中设立了几个就必须传几个,但是我们希望有些属性可以传也可以不传

```ts
function coordinate(coordinate: { x: number; y: number; z?: number }) {
  console.log(coordinate.x);
  console.log(coordinate.y);
  console.log(coordinate.z);
}

coordinate({ x: 12, y: 123 });
```

## 联合类型

简单的说就是或者

比如我们设立一个参数的类型为 number 或者是 string 类型,我们就可以使用联合类型

```ts
function foo(message: string | number) {
  console.log(message);
}

foo("abc");
foo(123);
```

传入给一个联合类型的值是非常简单的：只要保证是联合类型中的某一个类型的值即可

- 但是我们拿到这个值之后，我们应该如何使用它呢？因为它可能是任何一种类型。
- 比如我们拿到的值可能是 string 或者 number，我们就不能对其调用 string 上的一些方法；

那么我们怎么处理这样的问题呢？

- 我们需要使用**缩小**（narrow）联合（后续我们还会专门讲解缩小相关的功能）；
- TypeScript 可以根据我们缩小的代码结构，推断出更加具体的类型；

```ts
function foo(message: string | number) {
  if (typeof message === "string") {
    console.log(message.toString());
  } else {
    console.log(message);
  }
}

foo("abc");
foo(123);
```

联合类型的缺点:

- 需要进行很多的逻辑判断(类型缩小)
- 返回值的类型是不容易确定的

### 可选类型和联合类型的关系

其实上，可选类型可以看做是 类型 和 undefined 的联合类型：

```ts
function foo(name?: string) {
  console.log(name);
}

foo();
foo(undefined);
```

## 类型别名

在前面，我们通过在类型注解中编写 对象类型 和 联合类型，但是当我们想要多次在其他地方使用时，就要编写多
次。

比如我们可以给对象类型起一个别名：

```ts
type CoordinateType = {
  x: number;
  y: number;
  z?: number;
};
function coordinate(coordinate: CoordinateType) {
  console.log(coordinate.x);
  console.log(coordinate.y);
  console.log(coordinate.z);
}

coordinate({ x: 12, y: 123 });
```

## 类型断言

有时候 TypeScript 无法获取具体的类型信息，这个我们需要使用类型断言（Type Assertions）。

比如在开发中我们需要获取元素,通过 document.querySelector 获取元素的时候,ts 只知道函数会返回一个 Element,但是不知道具体的类型

我们明确需要获取 img 元素,img 可以设置 src 属性,但是默认的话我们是不能设置 src 的,因为 Element 类型上不存在 src 属性

![](/type-script/11.png)

这个时候我们就可以使用类型断言让它变成 imgElement

```ts
const imgEl = document.querySelector(".tao") as HTMLImageElement;
imgEl.src = "src地址";
```

TypeScript 只允许类型断言转换为 更具体 或者 不太具体 的类型版本，此规则可防止不可能的强制转换：

```ts
const name = "tao";
const age: number = (name as any) as number;
```

还有一个小案例可以看看:

```ts
class Person {}

class Student extends Person {
  say() {}
}

function sayHello(p: Person) {
  p.say();
}

const stu = new Student();
sayHello(stu);
```

sayHello 接收一个 Person 类型的参数

stu 是 Student 的实例,继承于 Person,当然可以传进去

本质上你还是 Student 类型,但是因为继承的原因,它在编译的时候会帮你转为 Person 类型

Person 里没有 say 方法,当然你调用的时候可以会报错啊

但是这不合情理,我是 Student 的实例,我不能使用我自己的方法,这就不行

这个时候我们就可以通过类型断言的方法把它变为 Student

```ts
class Person {}

class Student extends Person {
  say() {}
}

function sayHello(p: Person) {
  // p.say()
  (p as Student).say();
}

const stu = new Student();
sayHello(stu);
```

## 非空类型断言

当我们编写下面的代码时，在执行 ts 的编译阶段会报错：

- 这是因为传入的 name 有可能是为 undefined 的，这个时候是不能执行方法的；

```ts
function foo(name?: string) {
  console.log(name.toString());
}

foo("hello world");
```

但是，我们确定传入的参数是有值的，这个时候我们可以使用非空类型断言：

- 非空断言使用的是 ! ，表示可以确定某个标识符是有值的，跳过 ts 在编译阶段对它的检测；

```ts
function foo(name?: string) {
  console.log(name!.toString());
}

foo("hello world");
```

## 可选链

可选链事实上并不是 TypeScript 独有的特性，它是 ES11（ES2020）中增加的特性：

- 可选链使用可选链操作符 `?.`；
- 它的作用是当对象的属性不存在时，会短路，直接返回 undefined，如果存在，那么才会继续执行；

```ts
type Person = {
  name: string;
  friend?: {
    name: string;
  };
};

const info: Person = {
  name: "tao",
};

console.log(info.name);
console.log(info.friend.name);
```

虽然在编译的时候代码是没问题的,但是在代码执行的时候是会报错的

因为我们 info.friend 是 undefined,从 undefined 从取 name 属性,肯定是会报错的

因为我们之前说过,**错误发现的越早越好**,能在编译的时候发现错误当然是大于在执行的时候发现错误好

所以这里我们可以使用可选链

```ts
type Person = {
  name: string;
  friend?: {
    name: string;
  };
};

const info: Person = {
  name: "tao",
};

console.log(info.name);
console.log(info.friend?.name);
```

## ??和!!

### ??

它是 ES11 增加的新特性

空值合并操作符（??）是一个逻辑操作符，当操作符的左侧是 null 或者 undefined 时，返回其右侧操作数，
否则返回左侧操作数；

```ts
const name: string | null | undefined = null;
const message = name ?? "sandy";
console.log(message); // sandy
```

### !!

将一个其他类型转换成 boolean 类型；类似于 Boolean(变量)的方式；

```ts
const name = "tao";
let flag = !!name;
console.log(flag); // true
```

## 字面量

除了前面我们所讲过的类型之外，也可以使用字面量类型（literal types）：

```ts
// 其实你会发现你使用let和const定义变量是不同的
const message = "tao";
```

你肯定会以为 message 是一个 string 类型,其实它是一个'tao'类型(字面量类型)

![](/type-script/12.png)

其实使用 const 如果不指定类型的话,默认它会帮你转为字面量类型,当然你也可以为其它类型

在 ts 中你可以指定任何东西为类型

```ts
// 设置了字面量类型,它的值也只能是类型的值
let message: 123 = 123;
// 报错
message = "abc";
```

那你肯定会有疑问,这个字面量类型有什么意义喃,感觉没什么用啊

默认情况下这么做是没有太大的意义的，但是我们可以将多个类型联合在一起；

```ts
type AlignType = "left" | "right" | "center";

function changeAlign(align: AlignType) {
  console.log(`修改方向为${align}`);
}

changeAlign("left");
changeAlign("right");
changeAlign("center");
// 报错
changeAlign(311);
```

### 字面量推理

我们先来看一个案例:

```ts
type MethodType = "Get" | "Post";
function request(url: string, method: MethodType) {}

const options = {
  url: "http://www.codertao.work/",
  method: "Get",
};

//  这样肯定是会报错的,options.method是一个字符串类型
request(options.url, options.method);
```

我们当然有其它方法让它成功调用

```ts
// 给options设置类型(推荐)
type OptionsType = {
  url: string;
  method: MethodType;
};
const options: OptionsType = {
  url: "http://www.codertao.work/",
  method: "Get",
};

// 或者我们也可以进行类型断言
request(options.url, options.method as MethodType);

// 我们也可以使用官方的字面量推理
const options = {
  url: "http://www.codertao.work/",
  method: "Get",
} as const;
```

而且我们会发现通过字面量推理 options 中的属性是只读的了(后面会讲对象的相关知识)

![](/type-script/13.png)

## 类型缩小

什么是类型缩小呢？

- 类型缩小的英文是 Type Narrowing；
- 我们可以通过类似于 typeof padding === "number" 的判断语句，来改变 TypeScript 的执行路径；
- 在给定的执行路径中，我们可以缩小比声明时更小的类型，这个过程称之为 **缩小**;
- 而我们编写的 typeof padding === "number 可以称之为 **类型保护**（type guards）；

常见的类型保护有如下几种：

- typeof
- 平等缩小（比如===、!==）
- instanceof
- in
- 等等

### typeof

在 TypeScript 中，检查返回的值 typeof 是一种类型保护：因为 TypeScript 对如何 typeof 操作不同的值进行编码。

```ts
type MessageType = number | string;
function foo(message: MessageType) {
  if (typeof message === "string") {
    console.log(message.toString());
  } else {
    console.log(message);
  }
}
```

### 平等缩小

我们可以使用 Switch 或者相等的一些运算符来表达相等性（比如===, !==, ==, and != ）：

```ts
type Direction = "left" | "right" | "top" | "bottom";
function printDirection(direction: Direction) {
  // 1.if判断
  // if (direction === 'left') {
  //   console.log(direction)
  // } else if ()
  // 2.switch判断
  // switch (direction) {
  //   case 'left':
  //     console.log(direction)
  //     break;
  //   case ...
  // }
}
```

### instanceof

JavaScript 有一个运算符来检查一个值是否是另一个值的“实例”：

```ts
function printTime(time: string | Date) {
  if (time instanceof Date) {
    console.log(time.toUTCString());
  } else {
    console.log(time);
  }
}

class Student {
  studying() {}
}

class Teacher {
  teaching() {}
}

function work(p: Student | Teacher) {
  if (p instanceof Student) {
    p.studying();
  } else {
    p.teaching();
  }
}

const stu = new Student();
work(stu);
```

### in

Javascript 有一个运算符，用于确定对象是否具有带名称的属性：in 运算符

- 如果指定的属性在指定的对象或其原型链中，则 in 运算符返回 true；

```ts
type Fish = {
  swimming: () => void;
};

type Dog = {
  running: () => void;
};

function walk(animal: Fish | Dog) {
  if ("swimming" in animal) {
    animal.swimming();
  } else {
    animal.running();
  }
}

const fish: Fish = {
  swimming() {
    console.log("swimming");
  },
};

walk(fish);
```

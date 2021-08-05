---
title: 接口
---

## 接口的声明

在前面我们通过 type 可以用来声明一个对象类型：

对象的另外一种声明方式就是通过接口来声明：

```ts
// type InfoType = {
//   name: string;
//   age: number;
// };
// 在接口名称前面+I(算是一种规范)
interface IInfoType {
  name: string;
  age: number;
}
const info: InfoType = {
  name: "tao",
  age: 19,
  numebr: number,
};
```

他们在使用上的区别，我们后续再来说明。

接下来我们继续学习一下接口的其他特性。

## 可选属性

接口中我们也可以定义可选属性：

```ts
interface InfoType {
  name: string;
  age: number;
  friend?: {
    name: string;
  };
}
const info: InfoType = {
  name: "tao",
  age: 19,
  friend: {
    name: "sandy",
  },
};

console.log(info.friend?.name);
```

## 只读属性

接口中也可以定义只读属性：

- 这样就意味着我们再初始化之后，这个值是不可以被修改的；

```ts
interface InfoType {
  readonly name: string;
  age: number;
}
const info: InfoType = {
  name: "tao",
  age: 19,
};

info.name = "sandy"; // 不能赋值给'name'，因为它是一个只读的属性。
```

## 索引类型

前面我们使用 interface 来定义对象类型，这个时候其中的属性名、类型、方法都是确定的，但是有时候我们会遇
到类似下面的对象：

```ts
interface IFrontEnd {
  [index: number]: string;
}

const frontEnd: IFrontEnd = {
  0: "HTML",
  1: "CSS",
  2: "JavaScript",
  3: "Vue.js",
};

interface ILanguageYear {
  [name: string]: number;
}

const languageYear: ILanguageYear = {
  C: 1972,
  Java: 1995,
  JavaScript: 1996,
  TypeScript: 2014,
};
```

## 函数类型

前面我们都是通过 interface 来定义对象中普通的属性和方法的，实际上它也可以用来定义函数类型：

```ts
interface ISumFn {
  (num1: number, num2: number): number;
}

const sum: ISumFn = (num1, num2) => {
  return num1 + num2;
};
```

当然，除非特别的情况，还是推荐大家使用类型别名来定义函数：

```ts
type SumFnType = (num1: number, num2: number) => number;
```

## 接口继承

接口和类一样是可以进行继承的，也是使用 extends 关键字：

- 并且我们会发现，接口是支持多继承的（类不支持多继承）

```ts
interface IInfo {
  name: string;
}

interface IAge {
  age: number;
}

interface IPerson extends IInfo, IAge {
  say(): void;
}

const info: IPerson = {
  name: "tao",
  age: 19,
  say() {
    console.log("Hello World");
  },
};
```

## 接口的实现

接口定义后，也是可以被类实现的：

- 如果被一个类实现，那么在之后需要传入接口的地方，都可以将这个类传入；
- 这就是**面向接口**开发；

```ts
interface ISay {
  say: () => void;
}

interface IEat {
  eat: () => void;
}

function foo(bar: ISay) {}

// 继承:只能实现单继承
// 接口:可以实现多个接口
class Person implements ISay, IEat {
  say() {}
  eat() {}
}
```

```ts
// 编写一些共同的API就可以使用面向接口开发
// 面向接口开发属于面向对象中的概念
interface ISay {
  say: () => void;
}

interface IEat {
  eat: () => void;
}

class Person implements ISay, IEat {
  say() {}
  eat() {}
}

function foo(bar: Person) {
  bar.say();
  bar.eat();
}

// 所有实现了接口的类对应的对象,都是可以传入的
foo(new Person());
```

## interface 和 type 区别

我们会发现 interface 和 type 都可以用来定义对象类型，那么在开发中定义对象类型时，到底选择哪一个呢？

- 如果是定义非对象类型，通常推荐使用 type，比如 Direction、Alignment、一些 Function；

如果是定义对象类型，那么他们是有区别的：

- interface 可以重复的对某个接口来定义属性和方法；
- 而 type 定义的是别名，别名是不能重复的；

```ts
interface Person {
  name: string;
}

// 定义同名的多个接口的时候,接口内部的属性会进行合并
interface Person {
  age: number;
}
```

```ts
type Person {
  name: string;
}

// 报错 重复的标识符'Person'。
type Person {
  age: number;
}

```

当然官网文档中也有提到
::: warning 提示
For the most part, you can choose based on personal preference, and TypeScript will tell you if it needs something to be the other kind of declaration. If you would like a heuristic, use interface until you need to use features from type.

在大多数情况下，你可以根据**个人喜好**来选择，TypeScript 会告诉你，如果它需要的东西是其他类型的声明。如果你想要一个启发式的方法，请使用 interface，直到你需要使用来自 type 的特性。
:::

## 字面量赋值

和字面量类型大致相似,但还是有不同点

```ts
interface IPerson {
  name: string;
  age: number;
  say: () => void;
}

const info: IPerson = {
  name: 'tao',
  age: 19,
  say() {}
  // 报错,IPerson没有eat
  eat(){}
};


```

这是因为 TypeScript 在字面量直接赋值的过程中，为了进行类型推导会进行严格的类型限制。

但是之后如果我们是将一个 变量标识符 赋值给其他的变量时，会进行 freshness **擦除**操作。

```ts
interface IPerson {
  name: string;
  age: number;
  say: () => void;
}

const info = {
  name: "tao",
  age: 19,
  say() {},
  eat() {},
};

const p: IPerson = info;
console.log(p);
```

## 交叉类型

前面我们学习了联合类型：

- 联合类型表示多个类型中一个即可

还有另外一种类型合并，就是交叉类型（Intersection Types）：

- 交叉类似表示需要满足多个类型的条件；
- 交叉类型使用 & 符号；

我们来看下面的交叉类型：

- 表达的含义是 number 和 string 要同时满足；
- 但是有同时满足是一个 number 又是一个 string 的值吗？其实是没有的，所以 MyType 其实是一个 never 类型；

```ts
type MyType = number & string;
```

所以，在开发中，我们进行交叉时，通常是对对象类型进行交叉的：

就像接口的继承,当我们有多个接口需要组合的时候,可以使用接口继承的方法的,也可以使用交叉类型的方式

```ts
interface IInfo {
  name: string;
}

interface IAge {
  age: number;
}

type IPerson = IInfo & IAge;

const info: IPerson = {
  name: "tao",
  age: 19,
};
```

## 枚举类型

枚举类型是为数不多的 TypeScript 特性有的特性之一：

- 枚举其实就是将一组可能出现的值，一个个列举出来，定义在一个类型中，这个类型就是枚举类型；
- 枚举允许开发者定义一组命名常量，常量可以是数字、字符串类型；

```ts
enum Direction {
  LEFT,
  TOP,
  RIGHT,
  BOTTOM,
}

function turnDirection(direction: Direction) {
  switch (direction) {
    case Direction.LEFT:
      console.log("向左转动");
      break;
    case Direction.TOP:
      console.log("向上转动");
      break;
    case Direction.RIGHT:
      console.log("向右转动");
      break;
    case Direction.BOTTOM:
      console.log("向下转动");
      break;
    default:
      const myDirection: never = direction;
  }
}

turnDirection(Direction.LEFT); // 向左转动
turnDirection(Direction.TOP); // 向上转动
turnDirection(Direction.RIGHT); // 向右转动
turnDirection(Direction.BOTTOM); // 向下转动
```

### 枚举类型的值

枚举类型默认是有值的，比如上面的枚举，默认值是这样的：

```ts
enum Direction {
  LEFT = 0,
  TOP = 1,
  RIGHT = 2,
  BOTTOM = 3,
}
```

当然，我们也可以给枚举其他值：

- 这个时候会从 100 进行递增；

```ts
enum Direction {
  LEFT = 100,
  TOP = 1,
  RIGHT = 2,
  BOTTOM = 3,
}
```

我们也可以给他们赋值其他的类型：

```ts
enum Direction {
  LEFT = "LEFT",
  TOP = "TOP",
  RIGHT = "RIGHT",
  BOTTOM = "BOTTOM",
}
```

---
title: 数据类型
---

## JavaScript 类型

### number

:::tip 提示
小技巧: 在开发中能让它自己类型推导的就让它自己类型推导(这是我个人的开发习惯)
:::

数字类型是我们开发中经常使用的类型，TypeScript 和 JavaScript 一样，不区分整数类型（int）和浮点型
（double），统一为 number 类型。

```ts
let num = 123;
let num2 = 0.3;
```

如果你学习过 ES6 应该知道，ES6 新增了二进制和八进制的表示方法，而 TypeScript 也是支持二进制、八进制、十
六进制的表示：

```ts
// 十进制
let num = 100;
// 二进制
let num2 = 0b110;
// 八进制
let num3 = 0o110;
// 十六进制
let num4 = 0x100;

console.log(num); // 100
console.log(num2); // 6
console.log(num3); // 72
console.log(num4); // 256
```

### boolean

boolean 类型只有两个取值：true 和 false，

```ts
let flag = true;
flag = 10 > 20;
```

### string

string 类型是字符串类型，可以使用单引号或者双引号表示：

```ts
let name = "tao";
name = "sandy";
```

同时也支持 ES6 的模板字符串来拼接变量和字符串：

```ts
let say = `my name is ${name}`;
console.log(say); // my name is sandy
```

### array

数组类型的定义有两种方式：

```ts
let names = [];
```

这样是不好的,虽然你定义 names 是一个数组,但是无法明确 names 中的内容是什么类型的元素
而且,学过数组的也都知道,在数组中最好内容是同一种类型的元素,这个我就不赘述了

所以这里有两种方式来定义数组

```ts
let names: Array<string> = ["abc", "cba"];
let names2: string[] = ["abc", "cba"];
```

那肯定要问了,这两种方法有什么区别吗

在开发中我更**推荐**使用第二种方法

第一种方法当你在写 jsx 的时候,<>和上面的泛型<>(后面会讲),两个是一样的,babel 在解析的时候就不知道怎么解析了,就会出问题

所以我更推荐你使用第二种方法来定义数组

### object

object 对象类型可以用于描述一个对象：

```ts
let info: object = {
  name: "tao",
  age: 19,
};
```

但是如果注解了类型为 object 的话,我们是无法从对象上取得属性的

![](/type-script/6.png)

这里当然它会自己进行类型推导的,所以这里**推荐**不写注解,当然你写的话也可以,也有其它方法进行改造,这里还没有学,先不写,后面会提到

```ts
let info = {
  name: "tao",
  age: 19,
};
```

### null 和 undefined 类型 null 和 undefined 类型

在 JavaScript 中，undefined 和 null 是两个基本数据类型。

在 TypeScript 中，它们各自的类型也是 undefined 和 null，也就意味着它们既是实际的值，也是自己的类型：

```ts
let n = null;
let u = undefined;
```

但是在开发中如果你真的想定义 null 和 undefined 类型的话,我建议进行类型注解,因为在编译的时候,如果你没有进行类型注解的话,它会把这两个当做是 any 类型的(后面会将),然后我们就可以直接修改这两个的值,这当然是不好的

```ts
let n = null;
n = "123";

let u = undefined;
u = 23;
```

![](/type-script/7.png)

所以**建议**使用 null 和 undefined 的时候还是进行类型注解

```ts
let n: null = null;
let u: undefined = undefined;
```

![](/type-script/8.png)

### symbol

在 ES5 中，如果我们是不可以在对象中添加相同的属性名称的，比如下面的做法：

```ts
const info = {
  title: "coder",
  title: "teacher",
};
```

通常我们的做法是定义两个不同的属性名字：比如 title1 和 title2。

但是我们也可以通过 symbol 来定义相同的名称，因为 Symbol 函数返回的是不同的值：

```ts
const title1 = Symbol("title");
const title2 = Symbol("title");

const info = {
  [title1]: "coder",
  [title2]: "teacher",
};
```

## TypeScript 类型

### any

在某些情况下，我们确实无法确定一个变量的类型，并且可能它会发生一些变化，这个时候我们可以使用 any 类型（类似
于 Dart 语言中的 dynamic 类型）。

any 类型有点像一种讨巧的 TypeScript 手段：

- 我们可以对 any 类型的变量进行任何的操作，包括获取不存在的属性、方法；
- 我们给一个 any 类型的变量赋值任何的值，比如数字、字符串的值；

```ts
// 相当于定义了一个变量,没有指定初始值,那么默认就是一个any类型
let message;
message = 123;
message = "123";
message = {};
message.toString();
```

如果对于某些情况的处理过于繁琐不希望添加规定的类型注解，或者在引入一些第三方库时，缺失了类型注解，这个时候
我们可以使用 any

### unknown

unknown 是 TypeScript 中比较特殊的一种类型，它用于描述类型**不确定**的变量。

那下面我们来看一下这个场景

```ts
function foo() {
  return "abc";
}
function bar() {
  return 123;
}

let flag = true;
let result;
if (flag) {
  result = foo();
} else {
  result = bar();
}
```

当 flag 发生改变的时候,我们不确定最后我们拿到的 result 是一个什么样的类型

这里当然可以使用 any 或者|(联合类型,这里暂时不用,后面会讲),也可以注解为 unknown 类型

那这里我们是用 any 还是 unknown 喃?

我推荐用 unknown,这里来看看 any 和 unknown 的不同

unknown 类型只能赋值给 any 和 unknown 类型,any 类型可以赋值给任意类型

```ts
// 如果result是any的话,当然没有问题
// 如果result是unknown的话,这里是会报错的
// 你想,怎么可能把一个不确定的东西明确指定类型
// 推荐不使用any,any太灵活了
let message: string = result;
let num: number = result;
```

所以**推荐**不确定值的话使用 unknown 类型,当然我个人更喜欢联合类型(好看一点)(后面会讲)

### void

void 通常用来指定一个函数是没有返回值的，那么它的返回值就是 void 类型：

我们可以将 null 和 undefined 赋值给 void 类型，也就是函数可以返回 null 或者 undefined

```ts
function foo(): void {
  return undefined;
}

function bar(): void {
  return null;
}
```

void 在开发中一般是不写的,一般要指定返回类型就直接写返回类型

### never

never 表示**永远不会发生值**的类型，比如一个函数：

- 如果一个函数中是一个死循环或者抛出一个异常，那么这个函数会返回东西吗？
- 不会，那么写 void 类型或者其他类型作为返回值类型都不合适，我们就可以使用 never 类型；

```ts
function foo(): never {
  while (true) {}
}

function bar(): never {
  throw new Error();
}
```

never 有什么样的应用场景呢？这里我们举一个例子，但是它用到了联合类型，后面我们会讲到：

```ts
function logmessage(message: number | string) {
  switch (typeof message) {
    case "number":
      console.log("number-message");
      break;
    case "string":
      console.log("string-message");
      break;
    default:
      break;
  }
}

logmessage("abc");
logmessage(123);
```

这个代码是没有任何问题的,在我们想要调用的时候必须传入 number 类型或者 string 类型

如果当其它开发人员想调用这个函数想传入一个 boolean 类型,可能他会这样做

我这里想传入一个 boolean 类型,那么它会在参数的位置添加一个 | boolean

这样他就可以顺利传入一个 boolean 类型的值

从代码的角度看,可以运行没有问题,但是这样不严谨,我传入一个 boolean 类型的值,但是函数内没有对传入 boolean 类型的进行处理,且可以成功调用函数

这样肯定不是很好

那这里这样做就会让代码更加的严谨

```ts
function logmessage(message: number | string | boolean) {
  switch (typeof message) {
    case "number":
      console.log("number-message");
      break;
    case "string":
      console.log("string-message");
      break;
    default:
      const check: never = message;
      break;
  }
}
```

对默认值进行一个限制

这代码的意思表示,如果你想在我原来的基础上添加了一个新的类型,但是你又想直接调用函数是不能成功的,你必须在函数内写上传入新的类型需要处理的 case

解释:我添加了一个 boolean 类型,这样我就可以传入一个 boolean 类型了,当我调用的时候,因为函数内部没有处理关于传入 boolean 类型的值应该怎么做,就会跳到 default 上,default 上写着我把 message 赋值给了 check 变量,check 变量是一个永远不会有值的变量,message 肯定是你传入的值,当然是有值,有值的赋值给一个永远不会有值的,当然就会报错了,所以这个时候就必须让你写上传入 boolean 应该怎么做的程序,这样代码就会比较严谨

```ts
function logmessage(message: number | string | boolean) {
  switch (typeof message) {
    case "number":
      console.log("number-message");
      break;
    case "string":
      console.log("string-message");
      break;
    case "boolean":
      console.log("boolean-message");
      break;
    default:
      const check: never = message;
      break;
  }
}
```

### tuple

tuple 是元组类型，很多语言中也有这种数据类型，比如 Python、Swift 等。

```ts
// 如果开发中我们确实想不同的元素组合在一起
// 这个时候使用数组肯定是有很多的弊端的
const info: any[] = ["tao", 19, 1.85];
// 这样写age的类型是any,因为你从any数组中取出一个元素,那当然是any咯
const age = info[1];
console.log(age.length);
// 虽然可以age.length不会报错,但是这不严谨,数字没有length的属性
```

元祖可以理解为多个元素的组合

我们使用元祖的话就可以解决这个需求

```ts
const info: [string, number, number] = ["tao", 19, 1.85];
const age = info[1];
// 报错
console.log(age.length);
```

![](/type-script/9.png)

那么 tuple 在什么地方使用的是最多的呢？

- tuple 通常可以作为返回的值，在使用的时候会非常的方便；

用过 react 的,应该都知道 useState,那我们简单来封装一个 useState

案例运用了泛型(后面会讲)

```ts
function useState<T>(state: T) {
  let currentState = state;
  const changeState = (newState: T) => {
    state = newState;
  };
  const tuple: [T, (newState: T) => void] = [currentState, changeState];
  return tuple;
}

const [nameState, setnameState] = useState("tao");
const [ageState, setageState] = useState(19);
```

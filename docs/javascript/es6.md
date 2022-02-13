# ES6 新特性

## 一、字面量增强

ES6 中对 `对象字面量` 进行了增强，称之为 Enhanced object literals（增强对象字面量）。

字面量的增强主要包括下面几部分：

- 属性的简写：Property Shorthand
- 方法的简写：Method Shorthand
- 计算属性名：Computed Property Names

```js
var name = "tao";
var age = 18;

var obj = {
  // name: name,
  // age: age,
  // foo: function () {
  //   console.log('foo');
  // }
  // 属性简写
  name,
  age,
  // 方法简写
  foo() {
    console.log("foo");
  },
  // 这个可不是字面量增强哦，这个就是一个key为foo，value为箭头函数的属性
  foo: () => {
    console.log(this);
  },
  // 计算属性简写
  [name + 2]: "bar+2",
};

obj[name + 1] = "bar+1";
console.log(obj);
```

## 二、解构

ES6 中新增了一个从数组或对象中方便获取数据的方法，称之为解构 Destructuring。

我们可以划分为：

- 数组的解构
- 对象的解构

### 2.1 数组的解构

```js
var names = ["zs", "ls", "ww"];

// 基本解构
var [item1, item2, item3] = names;
console.log(item1, item2, item3);

// 解构后面的值
var [, itemb, itemc] = names;
console.log(itemb, itemc);

// 只解构一部分，其余的放入另外一个数组
var [item6, ...item] = names;
console.log(item6, item);

// 添加默认值
// 正常情况item13位undefined，可以给item13添加默认值（跟函数参数添加默认值有点像）
var [item10, item11, item12, item13 = "zl"] = names;
console.log(item10, item11, item12, item13);
```

### 2.2 对象的解构

```js
var info = {
  name: "tao",
  age: 18,
  height: 1.88,
};

// 基本解构
var { name, age, height } = info;
console.log(name, age, height);

// 对象解构可以是任意顺序（数组是按照索引进行解构的，但对象是通过key进行解构的）
var { age, height, name } = info;
console.log(age, height, name);

// 重命名解构的名称
var { name: newName } = info;
console.log(newName);

// 添加默认值
var { gender: newGender = "man" } = info;
console.log(newGender);
```

### 2.3 解构的应用场景

解构目前在开发中`使用是非常多的`：

- 比如在开发中拿到一个变量时，自动对其进行解构使用；
- 比如对函数的参数进行解构；

```js
var obj = {
  name: "tao",
  age: 18,
  height: 1.88,
};

function foo({ name, age }) {
  console.log(name, age);
}
foo(obj);
```

## 三、let/const

### 3.1 let/const 的基本使用

在 ES5 中我们声明变量都是使用的 var 关键字，从 ES6 开始新增了两个关键字可以声明变量：let、const

- let、const 在其他编程语言中都是有的，所以也并不是新鲜的关键字；
- 但是 let、const 确确实实给 JavaScript 带来一些不一样的东西；

let 关键字：从直观的角度来说，let 和 var 是没有太大的区别的，都是用于声明一个变量

const 关键字：

- const 关键字是 constant 的单词的缩写，表示`常量`、`衡量`的意思；
- 它表示保存的数据一旦被赋值，就不能被修改；
- 但是如果赋值的是引用类型，那么可以通过引用找到对应的对象，修改对象的内容；

```js
const name = "tao";
// Assignment to constant variable.
// name = 'sandy'

const info = {
  name: "tao",
  age: 18,
  heigth: 1.88,
};

info.name = "sandy";
console.log(info.name); // sandy
```

!>不是说，使用 const 定义的数据是不能修改的吗，为什么我可以对 info 进行修改

我画一下内存图你就明白了

![](https://gitee.com/itsandy/picgo-img/raw/master/JavaScript/const引用内存地址图.png)

**使用 const 声明的变量的值或者是内存地址是不能修改的**

这里的 info 指向的是一个对象，对象是有自己的内存地址的（比如 0x100），这里其实就是把对象的内存地址赋值给了 info（info 指向/引用着一个对象）

使用 const 就意味着你这里的内存地址是不能修改的

info = {},意味着我们要把新的一个对象的内存地址赋值给 info，显式是不行的

我们通过 info.name 的方式，其实是通过内存地址找到 info 里面的 name，然后对 name 进行修改（并没有修改内存地址）

!>注意：另外 let、const 不允许重复声明变量；

```js
let name = "tao";
// let name = "sandy";
const age = 18;
// const age = 21;
// Identifier 'name,age' has already been declared
```

### 3.2 let/const 作用域提升

let、const 和 var 的另一个重要区别是作用域提升：

- 我们知道 var 声明的变量是会进行作用域提升的；
- 但是如果我们使用 let 声明的变量，在声明之前访问会报错；

```js
console.log(name); // Cannot access 'name' before initialization
let name = "tao";
```

那么是不是意味着 name 变量只有在代码执行阶段才会创建的呢？

- 事实上并不是这样的，我们可以看一下 ECMA262 对 let 和 const 的描述；

详情：https://tc39.es/ecma262/ 下的 14.3.1

![](https://gitee.com/itsandy/picgo-img/raw/master/JavaScript/let和const的描述.png)

**这些变量在其包含的环境记录被实例化时被创建，但在变量的 LexicalBinding 被评估之前，不能以任何方式访问。**

我的理解是：在执行上下文解析的时候变量就会被创建出来，但是因为某种限制你不能访问到它，直到它被赋值

那么变量已经有了，但是不能被访问，是不是一种作用域的提升呢？

事实上维基百科并没有对作用域提升有严格的概念解释，那么我们自己从字面量上理解；

- 作用域提升：在声明变量的作用域中，如果这个变量可以在声明之前被访问，那么我们可以称之为作用域提升；
- 在这里，它虽然被创建出来了，但是不能被访问，我认为不能称之为作用域提升；

所以我的观点是 let、const `没有进行作用域提升`，但是会在`解析阶段被创建出来`。

### 3.3 Window 对象添加属性

我们知道，在全局通过 var 来声明一个变量，事实上会在 window 上添加一个属性：

- 但是 let、const 是不会给 window 上添加任何属性的。

那么我们可能会想这个变量是保存在哪里呢？

我们先回顾一下最新的 ECMA 标准中对执行上下文的描述[跳转](javascript/advanced/js-implementation?id=变量环境和记录)

也就是说我们声明的变量和环境记录是被添加到变量环境中的：

- 但是标准有没有规定这个对象是 window 对象或者其他对象呢？
- ECMA 制定了规范，说我们这里需要一个 VE，那是不是所有的 JS 引擎都要有一个 VE（这其实是不一定的，我只要实现你 ECMA 要求的功能就可以了）
- 那么 JS 引擎在解析的时候，其实会有自己的实现；
- 比如 v8 在实现的时候他把这些东西放在了一个叫做 variables 对象里面（类型 VariableMap） 这个其实是一个 hashmap ，利用 hashmap 来实现存储的。
- 那么 window 对象呢？而 window 对象是早期的 GO 对象，在最新的实现中 window 其实是浏览器添加的全局对象，并且
  一直保持了 window 和 var 之间值的相等性；

我画了一幅图（但是总体来说这个不是那么好理解的，我写的时候都感觉一知半解）

![](https://gitee.com/itsandy/picgo-img/raw/master/JavaScript/v8引擎最新的实现.png)

## 四、块级作用域

### 4.1 var 的块级作用域

在我们前面的学习中，JavaScript 只会形成两个作用域：全局作用域和函数作用域。

ES5 中放到一个代码块中定义的变量，外面是可以访问的：

```js
{
  var name = "tao";
}
console.log(name); // tao
```

### 4.2 let/const 的块级作用域

在 ES6 中新增了块级作用域，并且通过 let、const、function、class 声明的标识符是具备块级作用域的限制的：

```js
{
  let name = "tao";
  const age = 18;
  function foo() {
    console.log("foo");
  }
  class Perosn {}
}
// console.log(name);  // height is not defined
// console.log(age); // age is not defined
foo(); // 可以访问（这是因为不同浏览器都有自己的实现，大多是浏览器为了兼容以前的代码，让function没有块级作用域）
// var p = new Person  // Person is not defined
```

### 4.3 块级作用域的应用

我们来看一个简单的案例

获取多个按钮并监听对应的点击事件

```js
const btnEl = document.querySelectorAll("button");
console.log(btnEl);
for (var i = 0; i < btnEl.length; i++) {
  btnEl[i].onclick = () => {
    console.log(`第${i}个按钮发生了点击`);
  };
}
```

我们会发现无论点击了那个按钮，打印的都是第 4 个按钮发生了点击，这是为什么喃？

因为使用 var 定义的变量是没有块级作用域的，意思就是 i 变量是全局作用域下的 i，当 for 循环结束 i 的值为 4，在函数作用域打印 i 的时候，自己函数内没有 i，就会去上层作用域找，上层作用域是全局作用域，全局作用域的 i 是 4，所以不管点击那个按钮，打印的 i 都是 4

那在以前是怎么保证打印对应的按钮结果喃？

在以前我们可能会通过立即执行函数进行优化

```js
const btnEl = document.querySelectorAll("button");
for (var i = 0; i < btnEl.length; i++) {
  (function (n) {
    btnEl[n].onclick = () => {
      console.log(`第${n}个按钮发生了点击`);
    };
  })(i);
}
```

在 ES6 以后我们就可以直接通过 let 来确保对应的打印结果了

```js
const btnEl = document.querySelectorAll("button");
for (let i = 0; i < btnEl.length; i++) {
  btnEl[i].onclick = () => {
    console.log(`第${i}个按钮发生了点击`);
  };
}
```

这是因为 let 是有块级作用域的,当 for 循环执行完毕，我们点击的按钮的时候，onclick 函数的中 i 会通过上层作用域（块级作用域）找到 i，第一次 i 是 0，然后+1+1，不会找到全局的（全局压根就没有 i）

### 4.4 暂时性死区

在 ES6 中，我们还有一个概念称之为暂时性死区：（来源于社区）

- 它表达的意思是在同一作用域下，使用 let、const 声明的变量，在声明之前，变量都是不可以访问的；
- 我们将这种现象称之为 temporal dead zone（暂时性死区，TDZ）；

```js
console.log(name);
let name = "tao";

function foo() {
  console.log(name);
  let name = "tao";
}
foo();

{
  console.log(name);
  const name = "tao";
}

// ReferenceError: Cannot access 'name' before initialization
```

## 五、var、let、const 的选择

那么在开发中，我们到底应该选择使用哪一种方式来定义我们的变量呢？

对于 var 的使用：

- 我们需要明白一个事实，var 所表现出来的特殊性：比如作用域提升、window 全局对象、没有块级作用域等都是一些
  历史遗留问题；
- 其实是 JavaScript 在设计之初的一种语言缺陷；
- 当然目前市场上也在利用这种缺陷出一系列的面试题，来考察大家对 JavaScript 语言本身以及底层的理解；
- 但是在实际工作中，我们可以使用最新的规范来编写，也就是不再使用 var 来定义变量了；

对于 let、const：

- 对于 let 和 const 来说，是目前开发中推荐使用的；
- 我们会**优先推荐使用 const**，这样可以保证数据的安全性不会被随意的篡改；
- 只有当我们明确知道一个变量后续会需要被重新赋值时，这个时候再使用 let；
- 这种在很多其他语言里面也都是一种约定俗成的规范，尽量我们也遵守这种规范；

## 六、字符串模板基本使用

### 6.1 基本用法

在 ES6 之前，如果我们想要将字符串和一些动态的变量（标识符）拼接到一起，是非常麻烦和丑陋的（ugly）。

ES6 允许我们使用字符串模板来嵌入 JS 的变量或者表达式来进行拼接：

- 首先，我们会使用 `` 符号来编写字符串，称之为模板字符串；
- 其次，在模板字符串中，我们可以通过 ${expression} 来嵌入动态的内容；

```js
const name = "tao";
console.log("My name is " + name);

console.log(`My name is ${name}`);
```

这里推荐一个 vscode 的插件`Template String Converter`

详情：https://marketplace.visualstudio.com/items?itemName=meganrogge.template-string-converter

当"${"被键入时，将字符串转换为模板字符串。

### 6.2 标签模板字符串

此节跳转 React 专栏[点击跳转](react/css-in-js?id=五、es6-标签模板字符串)

## 七、函数的补充

### 7.1 函数的默认参数

在 ES6 之前，我们编写的函数参数是没有默认值的，所以我们在编写函数时，如果有下面的需求：

- 传入了参数，那么使用传入的参数；
- 没有传入参数，那么使用一个默认值；

在 ES6 之前我们可能会这样来写

```js
function foo(x, y) {
  x = x || "1";
  y = y || "2";
  console.log(x, y);
}
foo();
```

这样写其实缺点还是很明显的

- 写起来不方便，且可读性不好
- 容易产生 bug

```js
function foo(x, y) {
  x = x || "1";
  y = y || "2";
  console.log(x, y); // 1 2
}
// 逻辑|| 其实是前面的值为true的话，才会使用前面的值
// 传入0 和 '',这两个隐式转化会转化为false，所以就会使用后面的值
foo(0, "");
```

而在 ES6 中，我们允许给函数一个默认值：

```js
function foo(x = "1", y = "2") {
  console.log(x, y);
}
foo();
foo(0, "");
```

如果我们函数的参数是一个对象的话，也可以添加默认值，并且可以配合解构一起使用

```js
function foo(
  { name, age } = {
    name: "tao",
    age: 18,
    friends: ["zs", "ls", "ww"],
  }
) {
  // 函数参数的默认值是一个{ name, age, friends }，从对象中解构出name，age
  console.log(name, age);
}
foo();

function bar({ name = "tao", age = 18 } = {}) {
  // 函数参数的默认值是一个{}，从对象中解构出name,age，给name和age添加默认值
  console.log(name, age);
}
bar();
```

另外参数的默认值我们通常会将其放到最后（在很多语言中，如果不放到最后其实会报错的`规范`

但是 JavaScript 允许不将其放到最后，但是意味着还是会按照顺序来匹配；

```js
function foo(x, y, z = 30) {
  console.log(x, y, z);
}
foo(10, 20);
```

另外默认值会改变函数的 length 的个数，默认值以及后面的参数都不计算在 length 之内了

```js
function foo(a, b, c = 30, d) {
  console.log(foo.length); // 2
}
foo();
```

### 7.2 函数的剩余参数

ES6 中引用了 rest parameter，可以将不定数量的参数放入到一个数组中：

- 如果最后一个参数是 ... 为前缀的，那么它会将剩余的参数放到该参数中，并且作为一个数组；

```js
function foo(x, y, ...agrs) {
  console.log(x, y, agrs); // 10 20 [30,40,50]
}
foo(10, 20, 30, 40, 50);
```

那么剩余参数和 arguments 有什么区别呢？

- 剩余参数只包含那些没有对应形参的实参，而 arguments 对象包含了传给函数的所有实参；
- arguments 对象不是一个真正的数组，而 rest 参数是一个真正的数组，可以进行数组的所有操作；
- arguments 是早期的 ECMAScript 中为了方便去获取所有的参数提供的一个数据结构，而 rest 参数是 ES6 中提供
  并且希望以此来替代 arguments 的；

详情：[点击跳转](javascript/advanced/arguments?id=认识-arguments)

剩余参数**必须**放到最后一个位置，否则会报错。

## 八、展开语法

展开语法(Spread syntax)：

- 可以在函数调用/数组构造时，将数组表达式或者 string 在语法层面展开；
- 还可以在构造字面量对象时, 将对象表达式按 key-value 的方式展开；

详情：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Spread_syntax

展开语法的场景：

- 在函数调用时使用；
- 在数组构造时使用；
- 在构建对象字面量时，也可以使用展开运算符，这个是在 ES2018（ES9）中添加的新特性；（本来想写在 ES9 的文章中的，因为展开语法和剩余参数配合得多，就写在这里）

```js
// 1.函数调用
const names = ["zs", "ls", "ww"];
const name = "tao";
const info = { name: "tao", age: 18 };

function foo(...args) {
  console.log(args); // [ 'zs', 'ls', 'ww', 't', 'a', 'o' ]
}

foo(...names, ...name);

// 2.字面量数组构造或字符串
const newNames = [...names, ...name];
console.log(newNames); // [ 'zs', 'ls', 'ww', 't', 'a', 'o' ]

// 3.构造字面量对象时,进行克隆或者属性拷贝
const obj = { ...info, height: 1.88, ...names }; // { '0': 'zs', '1': 'ls', '2': 'ww', name: 'tao', age: 18, height: 1.88 }
console.log(obj);
```

!> 展开运算符其实是一种浅拷贝

```js
const info = {
  name: "tao",
  age: 18,
  friends: {
    name: "sandy",
    age: 21,
  },
};
const obj = { ...info, height: 1.88 };

info.friends.age = 53;
console.log(obj.friends.age); // 53
```

还是来画一下内存图吧

![](https://gitee.com/itsandy/picgo-img/raw/master/JavaScript/展开语法浅拷贝内存图.png)

## 九、数值的表示

```js
// 十进制
const num1 = 100;
// 二进制 b -> binary
const num2 = 0b100;
// 八进制 o => octonary
const num3 = 0o100;
// 十六进制 x = hex
const num4 = 0x100;
console.log(num1, num2, num3, num4); // 100 4 64 256
```

另外在 ES2021 新增特性：数字过长时，可以使用`_`作为连接符(只是为了方便查看)

```js
const num5 = 100_000_000_000_000;
console.log(num5); // 100000000000000
```

## 十、Symbol

Symbol 是什么呢？Symbol 是 ES6 中新增的一个基本数据类型，翻译为符号。

那么为什么需要 Symbol 呢？

- 在 ES6 之前，对象的属性名都是字符串形式，那么很容易造成**属性名的冲突**；
- 比如原来有一个对象，我们希望在其中**添加一个新的属性和值**，但是我们在不确定它原来内部有什么内容的情况下，
  **很容易造成冲突，从而覆盖掉它内部的某个属性**；
- 比如我们前面在讲 apply、call、bind 实现时，我们有给其中**添加一个 fn 属性**，那么如果它内部原来已经有了 fn 属性了
  呢？
- 比如开发中我们使用混入，那么混入中出现了同名的属性，必然有一个会被覆盖掉；

![](https://gitee.com/itsandy/picgo-img/raw/master/JavaScript/对象key属性底层.png)

Symbol 就是为了解决上面的问题，用来`生成一个独一无二的值`。

```js
const s1 = Symbol();
const s2 = Symbol();
console.log(s1 === s2); // false
```

- Symbol 值是通过 **Symbol 函数**来生成的，生成后可以**作为属性名**；
- 也就是在 ES6 中，对象的属性名可以使用**字符串**，也可以使用 **Symbol 值**；

Symbol 即使多次创建值，它们也是不同的：Symbol 函数执行后每次创建出来的值都是独一无二的；

我们也可以在创建 Symbol 值的时候传入一个描述 description：这个是 ES2019（ES10）新增的特性；

### 10.1 Symbol 作为属性名

我们通常会使用 Symbol 在对象中表示唯一的属性名：

```js
const s1 = Symbol();
const s2 = Symbol();
const s3 = Symbol();
const s4 = Symbol();
const s5 = Symbol();

// 1.直接定义字面量
const obj = {
  name: "tao",
  age: 18,
  [s1]: "aaa",
  [s2]: "bbb",
};

// 2.通过属性名赋值
obj[s3] = "ccc";
obj[s4] = "ddd";

// 3.通过Object.defineProperty
Object.defineProperty(obj, s5, {
  configurable: true,
  enumerable: true,
  writable: true,
  value: "eee",
});
console.log(obj);

// 4.我们也可以在创建Symbol值的时候传入一个描述description：这个是ES2019（ES10）新增的特性；

const s6 = Symbol("aaa");
console.log(s6.description); // aaa

// 5.获取Symbol属性(不能通过.语法获取)
console.log(obj[s2]); // bbb
console.log(obj.s2); // undefined(通过点语法会通过后面的属性（转为字符串）在对象中查找)

// 6.使用Symbol作为属性名是，在遍历/Object.key等方法是无法获取Symbol的值
console.log(Object.keys(obj)); // [ 'name', 'age' ]
console.log(Object.getOwnPropertyNames(obj)); // [ 'name', 'age' ]
console.log(Object.getOwnPropertySymbols(obj)); // [ Symbol(), Symbol(), Symbol(), Symbol(), Symbol() ]
// for (const sKey of obj) {
//   // TypeError: obj is not iterable
//   console.log(obj[sKey]);
// }
const sKeys = Object.getOwnPropertySymbols(obj);
for (const sKey of sKeys) {
  console.log(obj[sKey]); // aaa bbb ccc ddd eee
}
```

### 10.2 相同值的 Symbol

前面我们讲 Symbol 的目的是为了创建一个独一无二的值，那么如果我们现在就是想创建相同的 Symbol 应该怎么
来做呢？

- 我们可以使用 Symbol.for 方法来做到这一点；
- 并且我们可以通过 Symbol.keyFor 方法来获取对应的 key；

```js
const s1 = Symbol.for("aaa");
const s2 = Symbol.for("aaa");
const key = Symbol.keyFor(s1);
const s3 = Symbol.for(key);

console.log(s1 === s3); // true
```

## 十一、Set

在 ES6 之前，我们存储数据的结构主要有两种：数组、对象。

- 在 ES6 中新增了另外两种**数据结构**：Set、Map，以及它们的另外形式 WeakSet、WeakMap。

Set 是一个新增的数据结构，可以用来保存数据，类似于数组，但是和数组的区别是**元素不能重复**。

### 11.1 Set 的基本使用

创建 Set 我们需要`通过Set构造函数`（暂时没有字面量创建的方式）：

```js
// Set可以传入一个数组，数组中存放着不同的元素
const set = new Set([20, 30, 40, 20, 30]);
console.log(set); // { 20, 30, 40 }
```

我们可以发现 Set 中存放的元素是不会重复的，那么 Set 有一个非常常用的功能就是给数组去重。

以前没有 set 我们可能会自己写一个 for 循环来去重

```js
const array = [30, 40, 30, 50, 60, 50];
const newArray = [];
for (const item of array) {
  if (newArray.indexOf(item) === -1) {
    newArray.push(item);
  }
}
console.log(array); // [ 30, 40, 30, 50, 60, 50 ]
console.log(newArray); // [ 30, 40, 50, 60 ]
```

有了 set 之后我们就可以很快的进行数组去重

```js
const array = [30, 40, 30, 50, 60, 50];
const arraySet = new Set(array);
const newArr = Array.from(arraySet);
console.log(newArr); // [ 30, 40, 50, 60 ]
```

set 还支持扩展语法

```js
const array = [30, 40, 30, 50, 60, 50];
const arraySet = new Set(array);
const newArr = [...arraySet];
console.log(newArr); // [ 30, 40, 50, 60 ]
```

### 11.2 Set 的常见方法

Set 常见的属性：

- Set 常见的属性：

Set 常用的方法：

- add(value)：添加某个元素，返回 Set 对象本身；
- delete(value)：从 set 中删除和这个值相等的元素，返回 boolean 类型；
- has(value)：判断 set 中是否存在某个元素，返回 boolean 类型；
- clear()：清空 set 中所有的元素，没有返回值；
- forEach(callback, [, thisArg])：通过 forEach 遍历 set；

另外 Set 是支持 for of 的遍历的。

```js
const arrSet = new Set();
// add
arrSet.add(10);
arrSet.add(20);
arrSet.add(30);
arrSet.add(20);
arrSet.add(10);
console.log(arrSet); // { 10, 20, 30 }

// forEach
arrSet.forEach((item) => {
  console.log(item); // 10,20,30
});

// for of
for (const arr of arrSet) {
  console.log(arr); // 10,20,30
}

// delete
arrSet.delete(20);
arrSet.delete(30);
console.log(arrSet); // { 10 }

// has
console.log(arrSet.has(20)); // false

// clear
arrSet.clear();
console.log(arrSet); // {}
```

## 十二、WeakSet

### 12.1 WeakSet 的基本使用

和 Set 类似的另外一个数据结构称之为 WeakSet，也是内部元素不能重复的数据结构。

那么和 Set 有什么区别呢？

- 区别一：WeakSet 中只能存放对象类型，不能存放基本数据类型；
- 区别二：WeakSet 对元素的引用是**弱引用**，如果没有其他引用对某个对象进行引用，那么 GC 可以对该对象进行回收；

!>Set 建立的是强引用，weakSet 建立的是弱引用

那么有一个问题？什么是强引用，什么又是弱引用

我们先来看一段代码然后通过画图来分析一下

```js
const set = new Set();

let info = {
  name: "tao",
  age: 18,
  friends: {
    name: "sandy",
    age: 21,
  },
};

set.add(info);

info = null;
```

![](https://gitee.com/itsandy/picgo-img/raw/master/JavaScript/Set强引用.png)

那什么是弱引用喃？

```js
const weakSet = new WeakSet();

let info = {
  name: "tao",
  age: 18,
  friends: {
    name: "sandy",
    age: 21,
  },
};

weakSet.add(info);
info = null;
```

![](https://gitee.com/itsandy/picgo-img/raw/master/JavaScript/weakSet弱引用.png)

### 12.1 WeakSet 的应用场景

注意：**WeakSet 不能遍历**

- 因为 WeakSet 只是对对象的弱引用，如果我们遍历获取到其中的元素，那么有可能造成对象不能正常的销毁。
- 所以存储到 WeakSet 中的对象是没办法获取的；

那么这个东西有什么用呢？

- 事实上这个问题并不好回答，我们来使用一个 Stack Overflow 上的答案；
- 这个东西在开发中的应用场景也比较少

```js
let weakSet = new WeakSet();
class Person {
  constructor() {
    weakSet.add(this);
  }
  say() {
    if (!weakSet.has(this)) {
      throw "只能通过构造函数调用say方法";
    }
    console.log("Hello World");
  }
}
let p = new Person();
// 只能通过Person的实例来进行调用，不能通过像call啊等其它方法调用
p.say();
p.say.call({ name: "tao", age: 18 }); // throw '只能通过构造函数调用say方法'
```

那你可能会想这里为什么用 WeakSet 啊，用 Set 不行吗

用 Set 的话，如果有一天 p = null 了，因为 Set 是强引用，我们还需要还要设置 set = null（set 里还引用着 p 的引用）

使用 WeakSet 的话，p = null 的话，weakSet 就不需要设置为 null，因为 WeakSet 是弱引用，GC 会无视他，会把 p 的引用给回收掉的

## 十三、Map

Map，用于存储映射关系（键值对）

但是我们可能会想，在之前我们可以使用对象来存储映射关系，他们有什么区别呢？

- 事实上我们对象存储映射关系只能用字符串（ES6 新增了 Symbol）作为属性名（key）；
- 某些情况下我们可能希望通过其他类型作为 key，比如对象，这个时候会自动将对象转成字符串来作为 key；

```js
const tao = { name: "tao", age: 18 };
const sandy = { name: "sandy", age: 21 };
const person = { [tao]: "aaa", [sandy]: "bbb" };
console.log(person); // {[object Object]: 'bbb'}
```

你会发现我不是添加了两个键值对嘛，怎么只有后面那个，而且 key 为什么是 object object

这是因为 JavaScript 中的对象的 key 只能是字符串（除了 Symbol）

如果你添加的不是字符串，它内部会帮你做一个隐式转换的

tao 就是'[object,object]':'aaa'

sandy 转换成了'[object,object]':'bbb‘

因为两个 key 都是一样的，后面的则会覆盖前面的，所以结果就只有一个'[object,object]':'bbb‘

### 13.1 Map 的基本使用

如果我们确实想让一个 key 为对象的话，这个时候就可以使用 Map

```js
const tao = { name: "tao", age: 18 };
const sandy = { name: "sandy", age: 21 };
// map可以传入一个数组，数组中可以保存多个数组，内部数组存放着键值对
const map = new Map([
  [tao, "aaa"],
  [sandy, "bbb"],
]);
console.log(map);
/**
 * {
 *   { name: 'tao', age: 18 } => 'aaa',
 *   { name: 'sandy', age: 21 } => 'bbb'
 * }
 */
```

### 13.2 Map 的基本方法

Map 常见的属性：

- size：返回 Map 中元素的个数；

Map 常见的方法：

- set(key, value)：在 Map 中添加 key、value，并且返回整个 Map 对象；
- get(key)：根据 key 获取 Map 中的 value；
- has(key)：判断是否包括某一个 key，返回 Boolean 类型；
- delete(key)：根据 key 删除一个键值对，返回 Boolean 类型；
- clear()：清空所有的元素；
- forEach(callback, [, thisArg])：通过 forEach 遍历 Map；
  Map 也可以通过 for of 进行遍历。

```js
const map = new Map();
const info = { name: "tao" };
// set
map.set(info, 18);
map.set("aaa", "bbb");

console.log(map); // { { name: 'tao' } => 18, 'aaa' => 'bbb' }

// get
console.log(map.get("aaa")); // bbb

// has
console.log(map.has(info)); // true

// forEach
map.forEach((item, key, map) => {
  console.log(key, item, map); // { name: 'tao' } 18 Map(1) { { name: 'tao' } => 18
});

// for of
for (const item of map) {
  console.log(item); // [ { name: 'tao' }, 18 ]...
}
// 因为item拿到的是一个数组，我们也可以对其进行解构
for (const [key, value] of map) {
  console.log(key, value); // { name: 'tao' } 18   aaa bbb
}

// delete
map.delete(info);
console.log(map); // {'aaa' => 'bbb'}

// clear
map.clear();
console.log(map); // {size: 0}
```

## 十四、WeakMap

和 Map 类型相似的另外一个数据结构称之为 WeakMap，也是以键值对的形式存在的。

那么和 Map 有什么区别呢？

- 区别一：WeakMap 的 key 只能使用对象，不接受其他的类型作为 key；
- 区别二：WeakMap 的 key 对对象想的引用是弱引用，如果没有其他引用引用这个对象，那么 GC 可以回收该对象；

```js
const weakMap = new WeakMap();
weakMap.set(1, "aaa");
// TypeError: Invalid value used as weak map key
```

### 14.1 WeakMap 的基本使用

WeakMap 常见的方法有四个：

- set(key, value)：在 Map 中添加 key、value，并且返回整个 Map 对象；
- get(key)：根据 key 获取 Map 中的 value；
- has(key)：判断是否包括某一个 key，返回 Boolean 类型；
- delete(key)：根据 key 删除一个键值对，返回 Boolean 类型；

```js
const weakMap = new WeakMap();
const info = {
  name: "tao",
  age: 18,
};
weakMap.set(info, "aaa");
console.log(weakMap.get(info)); // aaa
console.log(weakMap.has(info)); // true
weakMap.delete(info);
console.log(weakMap); // WeakMap { <items unknown> }

// 这里补充一个小知识
// 为什么打印出的WeakMap是一个items unknow这个东西
// 因为WeakMap这个东西是不能遍历的，我们打印的时候本质上是会把它打印成字符串的(就跟我们打印一个对象一样)，它会隐式转为字符串然后打印出来，就看到的是这个样子
```

注意：WeakMap 也是不能遍历的

因为没有 forEach 方法，也不支持通过 for of 的方式进行遍历；

### 14.2 WeakMap 的应用场景

其实在 vue3 的响应式原理中就存在 WeakMap

那我们来简单说一下

比如这里有个 info，我们把它理解为 v2 data 这种返回的对象

当我们修改 info 里的 name 的时候，其实 template 里的每个标签，本质就是一行行的 render 函数，当 data 的值改变的时候，当对应依赖的值发生改变的时候，就会执行某几个函数，那么利用最新的 DOM 然后生成最新的虚拟 DOM，最后进行 diff 算法然后渲染到界面上

那么我们如何让一个东西发生改变，然后触发对应的函数喃(映射)，这个时候就可以用到 WeakMap

```js
let info = {
  name: "tao",
  age: 18,
};

function changeNamefn1() {
  console.log("changeNamefn1");
}
function changeNamefn2() {
  console.log("changeNamefn2");
}
function changeAgefn1() {
  console.log("changeAgefn1");
}
function changeAgefn2() {
  console.log("changeAgefn2");
}

// 这里使用WeakMap是因为如果弱引用，如果有一天info指向null了，weakMap是弱引用，不会影响GC把info回收掉的
const weakMap = new WeakMap();
// 这里为什么使用Map，因为我们收集的key都是一些字符串（基本数据类型），WeakMap需要的是一个对象
const map = new Map();

// 对info进行收集
map.set("name", [changeNamefn1, changeNamefn2]);
map.set("age", [changeAgefn1, changeAgefn2]);
weakMap.set(info, map);

// 数据发生改变
info.name = "sandy";
// 通过Proxy/Object.defineProperty进行监听
const target = weakMap.get(info);
const fns = target.get("name");
fns.forEach((fn) => fn());
```

## 十五、Proxy

### 15.1 监听对象的操作

我们先来看一个需求：有一个对象，我们希望监听这个对象中的属性被设置或获取的过程

- 通过我们前面所学的知识，能不能做到这一点呢？
- 其实是可以的，我们可以通过之前的属性描述符中的存储属性描述符来做到；

下面这段代码就利用了前面讲过的 Object.defineProperty 的存储属性描述符来对
属性的操作进行监听。

```js
const obj = {
  name: "tao",
  age: 18,
};

Object.keys(obj).forEach((key) => {
  let value = obj[key];
  Object.defineProperty(obj, key, {
    get() {
      console.log(`${key}属性的值被获取了`);
      return value;
    },
    set(newValue) {
      console.log(`${key}属性的值被设置了`);
      value = newValue;
    },
  });
});

obj.name = "sandy"; // name属性的值被设置了
obj.age = 21; // age属性的值被设置了
console.log(obj.name); // name属性的值被获取了 sandy
console.log(obj.age); // age属性的值被获取了  21
```

但是这样做有什么缺点呢？

- 首先，Object.defineProperty 设计的初衷，不是为了去监听截止一个对象中
  所有的属性的。
  - 我们在定义某些属性的时候，初衷其实是定义普通的属性，但是后面我们强
    行将它变成了数据属性描述符。
- 其次，如果我们想监听更加丰富的操作，比如新增属性、删除属性，那么
  Object.defineProperty 是无能为力的。
- 所以我们要知道，存储数据描述符设计的初衷并不是为了去监听一个完整的对
  象。

### 15.2 Proxy 的基本使用

在 ES6 中，新增了一个 Proxy 类，这个类从名字就可以看出来，是用于帮助我们创建一个代理的：

- 也就是说，如果我们希望监听一个对象的相关操作，那么我们可以先创建一个代理对象（Proxy 对象）；
- 之后对该对象的所有操作，都通过代理对象来完成，代理对象可以监听我们想要对原对象进行哪些操作（在 handler 里通过**捕获器**进行箭头）；

详情：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy

我们可以将上面的案例用 Proxy 来实现一次：

```js
const obj = {
  name: "tao",
  age: 18,
};
const objProxy = new Proxy(obj, {
  // 属性读取操作的捕捉器
  get(target, property) {
    console.log(target, `${property}的值被获取了`);
    return target[property];
  },
  // 属性设置操作的捕捉器
  set(target, property, value) {
    console.log(target, `的${property}的值被设置了`);
    target[property] = value;
  },
});

objProxy.name = "sandy"; // { name: 'tao', age: 18 } 的name的值被设置了
objProxy.age = 21; //  { name: 'sandy', age: 18 } 的age的值被设置了
console.log(objProxy.name); // { name: 'sandy', age: 21 } name的值被获取了 sandy
console.log(objProxy.age); //  { name: 'sandy', age: 21 } age的值被获取了 21
```

### 15.3 Proxy 的其他捕获器

Proxy 上有着非常多的捕获器，这里只举例一些捕获器，更多捕获器详情前往: https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy

```js
const obj = {
  name: "tao",
  age: 18,
};
const objProxy = new Proxy(obj, {
  // in 操作符的捕捉器
  has(target, prop) {
    console.log(`监听到${prop}属性使用了in操作`);
    return prop in target;
  },
  // delete 操作符的捕捉器
  deleteProperty(target, property) {
    console.log("监听到", target, `的${property}属性被删除了`);
    return delete target[property];
  },
});

console.log("name" in objProxy); // true
delete objProxy.age; // 监听到 { name: 'tao', age: 18 } 的age属性被删除了
console.log(objProxy); // { name: 'tao' }
```

## 十六、Reflect

Reflect 也是 ES6 新增的一个 API，它是一个`对象`，字面的意思是`反射`。

`那么这个Reflect有什么用呢`？

- 它主要提供了很多**操作 JavaScript 对象的方法**，有点像**Object 中操作对象的方法**；
- 比如 Reflect.getPrototypeOf(target)类似于 Object.getPrototypeOf()；
- 比如 Reflect.defineProperty(target, propertyKey, attributes)类似于 Object.defineProperty() ；

如果我们有 Object 可以做这些操作，那么`为什么还需要有Reflect这样的新增对象`呢？

- 这是因为在早期的 ECMA 规范中没有考虑到这种**对 对象本身 的操作如何设计会更加规范**，所以**将这些 API 放到了 Object 上面**；
- 但是**Object 作为一个构造函数**，这些操作实际上**放到它身上并不合适**；
- 另外还包含一些**类似于 in、delete 操作符**，让 JS 看起来是会有一些奇怪的；
- 所以在 ES6 中**新增了 Reflect**，让我们这些操作都集中到了 Reflect 对象上；

那么 Object 和 Reflect 对象之间的 API 关系，可以参考 MDN 文档：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/Comparing_Reflect_and_Object_methods

### 16.1 Reflect 的基本使用

虽然之前我们并没有直接操作对象，使用 Proxy 来对对象进行操作，但是你会发现最终我们还是在对原来的对象进行操作（比如 return target[property]）

其实我们使用 Reflect 来做

详情：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect

```js
const obj = {
  name: "tao",
  age: 18,
};
const objProxy = new Proxy(obj, {
  // 属性读取操作的捕捉器
  get(target, property) {
    console.log(target, `${property}的值被获取了`);
    return Reflect.get(target, property);
  },
  // 属性设置操作的捕捉器
  set(target, property, value) {
    console.log(target, `的${property}的值被设置了`);
    Reflect.set(target, property, value);
  },
});

objProxy.name = "sandy"; // { name: 'tao', age: 18 } 的name的值被设置了
objProxy.age = 21; //  { name: 'sandy', age: 18 } 的age的值被设置了
console.log(objProxy.name); // { name: 'sandy', age: 21 } name的值被获取了 sandy
console.log(objProxy.age); //  { name: 'sandy', age: 21 } age的值被获取了 21
```

### 16.2 receiver 的作用

我们先来看这段代码

```js
const obj = {
  _name: "tao",
  get name() {
    return this._name;
  },
  set name(newValue) {
    this._name = newValue;
  },
};
const objProxy = new Proxy(obj, {
  get(target, property) {
    return Reflect.get(target, property);
  },
  set(target, property, value) {
    Reflect.set(target, property, value);
  },
});

objProxy.name = "sandy";
objProxy.age = 21;
console.log(objProxy.name);
console.log(objProxy.age);
```

其实上面的代码我们还是在对原对象进行操作

我们设置 objProxy 的值的时候，其实会进入到 objProxy 的 set 捕获器里，然后调用 Reflect 的 set 方法，如果原对象里有 set 和 get 方法，我们最终是调用里面的 get/set 方法

前面我们使用在使用 getter 和 setter 的时候其实还有第三个参数叫做 receiver，它的作用是什么呢？

如果我们的源对象（obj）有 setter、getter 的访问器属性，那么可以通过 receiver 来改变里面的 this；

```js
const obj = {
  _name: "tao",
  get name() {
    return this._name;
  },
  set name(newValue) {
    this._name = newValue;
  },
};
const objProxy = new Proxy(obj, {
  get(target, property, receiver) {
    return Reflect.get(target, property, receiver);
  },
  set(target, property, value, receiver) {
    Reflect.set(target, property, value, receiver);
  },
});

objProxy.name = "sandy";
objProxy.age = 21;
console.log(objProxy.name);
console.log(objProxy.age);
```

### 16.3 construct 的作用

我们来看下面这段代码

```js
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  say() {
    console.log(this.name + " ~say hello");
  }
}

class Student {}
```

我提一个无理的要求，我想创建一个 Person 的实例，执行 person 里的代码，但是创建出来的是 Student 的实例

这其实是很难做到的，但是利用 Reflect 的 construct 方法就可以做到

详情：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect/construct

```js
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  say() {
    console.log(this.name + " ~Person say");
  }
}

class Student {
  say() {
    console.log(this.name + " ~Student say");
  }
}

const obj = Reflect.construct(Person, ["tao", 18], Student);
console.log(obj); // Student { name: 'tao', age: 18 }
obj.say(); // tao ~Student say
```

## 十七、Promise

### 17.1 异步任务的处理

在讲解 Promise 之前，我们从一个实际的例子来作为切入点

- 我们调用一个函数，这个函数中发送网络请求（我们可以用定时器来模拟）；
- 如果发送网络请求成功了，那么告知调用者发送成功，并且将相关数据返回过去；
- 如果发送网络请求失败了，那么告知调用者发送失败，并且告知错误信息；

```js
function foo(url, successCallback, failureCallback) {
  setTimeout(() => {
    if (url === "tao") {
      successCallback("url传入成功");
    } else {
      failureCallback("url传入失败");
    }
  }, 1000);
}

foo(
  "tao",
  (res) => {
    console.log(res);
  },
  (err) => {
    console.log(err);
  }
);
foo(
  "sandy",
  (res) => {
    console.log(res);
  },
  (err) => {
    console.log(err);
  }
);
```

### 17.2 了解 Promise

在上面的解决方案中，我们确确实实可以解决请求函数得到结果之后，获取到对应的回调，但是它存在两个主要的
问题：

1.  我们需要自己来设计回调函数、回调函数的名称、回调函数的使用等；
2.  对于不同的人、不同的框架设计出来的方案是不同的，那么我们必须耐心去看别人的源码或者文档，以
    便可以理解它这个函数到底怎么用；

我们来看一下 Promise 的 API 是怎么样的：

- Promise 是一个类，可以翻译成 承诺、许诺 、期约（当然我觉得还是不翻译比较好）；
- 当我们需要给予调用者一个承诺：待会儿我会给你回调数据时，就可以创建一个 Promise 的对象；
- 在通过 new 创建 Promise 对象时，我们需要传入一个回调函数，我们称之为**executor**
  - 这个回调函数会被立即执行，并且给传入另外两个回调函数 resolve、reject；
  - 当我们调用 resolve 回调函数时，会执行 Promise 对象的 then 方法传入的回调函数；
  - 当我们调用 reject 回调函数时，会执行 Promise 对象的 catch 方法传入的回调函数；

详情：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise

### 17.3 Promise 的代码结构

```js
const foo = new Promise((resolve, reject) => {
  // 调用resolve就会执行then传入的回调
  resolve("aaa");
  // 调用reject就会执行catch传入的回调
  reject("bbb");
});

foo.then((res) => {
  console.log(res);
});

foo.catch((err) => {
  console.log(err);
});

// aaa
```

上面 Promise 使用过程，我们可以将它划分成三个状态：

- 待定（pending）: 初始状态，既没有被兑现，也没有被拒绝；
  - 待定（pending）: 初始状态，既没有被兑现，也没有被拒绝；
- 已兑现（fulfilled）: 意味着操作成功完成；
  - 执行了 resolve 时，处于该状态；
- 已拒绝（rejected）: 意味着操作失败；
  - 执行了 reject 时，处于该状态；

### 17.4 Promise 的基本使用

那么有了 Promise，我们就可以将之前的代码进行重构了：

```js
function foo(url) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (url === "tao") {
        resolve("url传入成功");
      } else {
        reject("url传入失败");
      }
    }, 1000);
  });
}

foo("tao").then((res) => {
  console.log(res);
});

foo("tao").catch((res) => {
  console.log(res);
});
```

### 17.5 三种状态

Executor 是在创建 Promise 时需要传入的一个回调函数，这个回调函数会被立即执行，并且传入两个参数：

```js
new Promise((resolve, reject) => {
  console.log("Executor代码");
});
```

通常我们会在 Executor 中确定我们的 Promise 状态：

- 通过 resolve，可以兑现（fulfilled）Promise 的状态，我们也可以称之为已决议（resolved）（不可逆）；
- 通过 reject，可以拒绝（reject）Promise 的状态；

这里需要注意：一旦状态被确定下来，Promise 的状态会被 锁死，该 Promise 的状态是不可更改的

- 在我们调用 resolve 的时候，如果 resolve 传入的值本身不是一个 Promise，那么会将该 Promise 的状态变成 兑
  现（fulfilled）；
- 在之后我们去调用 reject 时，已经不会有任何的响应了（并不是这行代码不会执行，而是无法改变 Promise 状
  态）；

```js
const p = new Promise((resolve, reject) => {
  console.log("aaa");
  resolve("bbb");
  console.log("ccc");
  reject("ddd");
});

p.then((res) => {
  console.log("res", res);
});

p.catch((err) => {
  console.log("err", err);
});

// aaa
// ccc
// res bbb
```

### 17.6 resolve 参数

情况一:如果 resolve 传入一个普通的值或者对象，那么这个值会作为 then 回调的参数

```js
const p = new Promise((resolve, reject) => {
  resolve({ name: "tao", age: 18 });
});

p.then((res) => {
  console.log("res", res);
});

// { name: 'tao', age: 18 }
```

情况二:如果 resolve 中传入的是另外一个 Promise，那么这个新 Promise 会决定原 Promise 的状态

```js
const p = new Promise((resolve, reject) => {
  reject("aaa");
});

const p2 = new Promise((resolve, reject) => {
  resolve(p);
});

p.catch((err) => {
  console.log("err1", err);
});

p2.then((res) => {
  console.log("res", res);
});

p2.catch((err) => {
  console.log("err2", err);
});

// err1 aaa
// err2 aaa
```

情况三:如果 resolve 中传入的是一个对象，并且这个对象有实现 then 方法，那么会执行该 then 方法，并且根据
then 方法的结果来决定 Promise 的状态

```js
const info = {
  then(resolve, reject) {
    reject("bbb");
  },
};

const p = new Promise((resolve, reject) => {
  resolve(info);
});

p.then((res) => {
  console.log("res", res);
});

p.catch((err) => {
  console.log("err", err);
});

// err bbb
```

### 17.7 Promise 对象方法

#### 17.7.1 then 方法

then 方法是 Promise 对象上的一个方法：它其实是放在 Promise 的原型上的 Promise.prototype.then

详情：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/then

then 方法接受两个参数：

- fulfilled 的回调函数：当状态变成 fulfilled 时会回调的函数；
- reject 的回调函数：当状态变成 reject 时会回调的函数；

```js
const p = new Promise((resolve, reject) => {
  reject("aaa");
});

p.then(
  (res) => {
    console.log("res", res);
  },
  (err) => {
    console.log("err", err);
  }
);

// err aaa
```

当 executor 抛出异常时，是会调用 reject 函数的

```js
const p = new Promise((resolve, reject) => {
  throw Error("抛出了异常");
});

p.then(
  (res) => {
    console.log("res", res);
  },
  (err) => {
    console.log("err", err);
  }
);
// err Error: 抛出了异常
```

一个 Promise 的 then 方法是可以被多次调用的：

- 每次调用我们都可以传入对应的 fulfilled 回调；
- 当 Promise 的状态变成 fulfilled 的时候，这些回调函数都会被执行；

```js
const p = new Promise((resolve, reject) => {
  reject("aaa");
});

p.then(
  (res) => {
    console.log("res1", res);
  },
  (err) => {
    console.log("err1", err);
  }
);

p.then(
  (res) => {
    console.log("res2", res);
  },
  (err) => {
    console.log("err2", err);
  }
);

p.then(
  (res) => {
    console.log("res3", res);
  },
  (err) => {
    console.log("err3", err);
  }
);

// err1 aaa
// err2 aaa
// err3 aaa
```

then 方法本身是有返回值的，它的返回值是一个 Promise

- 但是 then 方法返回的 Promise 到底处于什么样的状态呢？

Promise 有三种状态，那么这个 Promise 处于什么状态呢？

- 当 then 方法中的回调函数本身在执行的时候，那么它处于 pending 状态；
- 当 then 方法中的回调函数返回一个结果时，那么它处于 fulfilled 状态，并且会将结果作为 resolve 的参数；
  - 情况一：返回一个普通的值；
  - 情况二：返回一个 Promise；
  - 情况三：返回一个 thenable 值；
- 当 then 方法抛出一个异常时，那么它处于 reject 状态；

```js
const p = new Promise((resolve, reject) => {
  resolve("aaa");
});

const p2 = p.then((res) => {
  console.log(res); // aaa
  // 这里没有返回任何值 = return undefined
  // then内部会返回一个Promise对象（返回的值会作为返回的新的Promise的resolve值）
});

// 相对于上面的代码会变成这样
// const p2 = p.then(res => {
// console.log(res);
//   return new Promise(resolve => {
//     resolve(undefined)
//   })
// })

// 返回一个Promise对象，所以我们可以对p2调用then方法
p2.then((res) => {
  console.log(res); // undefined
});

console.log(p2); // Promise
```

当然我们可以主动返回一个值，如果返回的是一个普通的值

```js
const p = new Promise((resolve, reject) => {
  resolve("aaa");
});

// 我们其实是可以进行链式调用的
// 其实是对第一个then方法的返回的Promise调用then方法（也就是p2），不是对调用前面的p进行调用
const p2 = p
  .then((res) => {
    return "bbb";
  })
  .then((res) => {
    console.log(res); // bbb
  });

// 等价于
// const p2 = p.then(res => {
//   console.log('res1', res);
// })

// p2.then(res => {
//   console.log('res2', res);
// })

// 所以你会发现这里打印的值是bbb，如果是对p调用then方法的话，打印的值应该是aaa
```

如果返回的是一个 Promise 的话,Promise 的 resolve 的参数值会作为返回的新的 Promise 的 resolve 的值

```js
const p = new Promise((resolve, reject) => {
  resolve("aaa");
});
const p2 = new Promise((resolve, reject) => {
  resolve("bbb");
});

p.then((res) => {
  console.log(res); // aaa
  return p2;
}).then((res) => {
  console.log(res); // bbb
});
```

如果返回的是一个对象，而且对象实现了 thenable，那么 resolve 的参数值会作为返回的新的 Promise 的 resolve 的值

```js
const p = new Promise((resolve, reject) => {
  resolve("aaa");
});

p.then((res) => {
  console.log(res); // aaa
  return {
    then(resolve, reject) {
      resolve("bbb");
    },
  };
}).then((res) => {
  console.log(res); // bbb
});
```

!>其实跟我们前面讲的 resolve 三种参数是一样的

#### 17.1.2 catch 方法

catch 方法也是 Promise 对象上的一个方法：它也是放在 Promise 的原型上的 Promise.prototype.catch

详情：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch

一个 Promise 的 catch 方法是可以被多次调用的：

- 每次调用我们都可以传入对应的 reject 回调；
- 当 Promise 的状态变成 reject 的时候，这些回调函数都会被执行；

```js
const p = new Promise((resolve, reject) => {
  reject("aaa");
});

p.catch((err) => {
  console.log("err1", err);
});

p.catch((err) => {
  console.log("err2", err);
});

// err1 aaa
// err2 aaa
```

事实上 catch 方法也是会返回一个 Promise 对象的，所以 catch 方法后面我们可以继续调用 then 方法或者 catch 方法：

那么我想问的是下面的打印结果是 catch 中的 err2 打印，还是 then 中的 res 打印呢？

答案是 res 打印，这是因为 catch 传入的回调在执行完后，默认状态依然会是 fulfilled 的；

catch 返回的 Promise 跟 then 返回的 Promise 规则是一样的，返回的值就会作为返回的 Promise 的 resolve 的值

```js
const p = new Promise((resolve, reject) => {
  reject("aaa");
});

p.catch((err) => {
  console.log("err1", err);
})
  .catch((err) => {
    console.log("err2", err);
  })
  .then((res) => {
    console.log("res", res);
  });

// catch也会返回一个Promise
// 等价于

// p.catch(err => {
//   console.log('err1', err);
//   // return undefined
//   return new Promise((resolve, reject) => {
//     // 返回的Promise调用resolve就会执行then传入的回调
//     // 所以会执行后面的then，不会执行后面的catch
//     resolve(undefined)
//   })
// }).then(res => {
//   console.log('res', res);
// })

// err1 aaa
// res undefined
```

那么如果我们希望后续继续执行 catch，那么需要抛出一个异常：

```js
const p = new Promise((resolve, reject) => {
  reject("aaa");
});

p.catch((err) => {
  console.log("err1", err);
})
  .catch((err) => {
    console.log("err2", err);
  })
  .then((res) => {
    console.log("res", res);
    throw new Error("抛出异常");
  })
  .catch((err) => {
    console.log("err3", err);
  });

// err1 aaa
// res undefined
// err3 Error: 抛出异常
```

#### 17.1.3 finally

finally 是在`ES9`（ES2018）中新增的一个特性：表示无论 Promise 对象无论变成 fulfilled 还是 reject 状态，最终都会
被执行的代码。

详情：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/finally

finally 方法是不接收参数的，因为无论前面是 fulfilled 状态，还是 reject 状态，它**都会执行**。

```js
const p = new Promise((resolve, reject) => {
  reject("aaa");
});

p.then((res) => {
  console.log("res", res);
})
  .catch((err) => {
    console.log("err", err);
  })
  .finally(() => {
    console.log("finally");
  });

// err aaa
// finally
```

finally 方法确实是执行了，但是你会好奇，为什么这里的 catch 方法执行了，

catch 这个东西比较特殊，当你这样写的时候

```js
const p = new Promise((resolve, reject) => {
  reject("aaa");
});

p.then((res) => {
  console.log("res", res);
}).catch((err) => {
  console.log("err", err);
});

// err aaa
```

通过我们前面的学习，链式调用的话我们知道 catch 方式其实是执行 then 方法返回的新的 Promise 里的 reject 执行后的回调

我 catch 方法不是调用 then 方法返回的 Promise 吗？（我这里 then 方法压根不会执行，那你 catch 为什么执行了）

简单来说：

这样写 catch 方法其实它内部在实现的时候会先看你上面这里(p)最有没有执行 reject 或者抛出异常，如果没有，才会去看前面 then 返回的 Promise 里有没有对应的 reject 执行或者是抛出异常（优先级的顺序）

相当于 catch 就是 then 方法第二个参数的语法糖

通过 catch 方法来传入错误（拒绝）捕获的回调函数

```js
const p = new Promise((resolve, reject) => {
  throw Error("抛出了异常");
});

p.catch((err) => {
  console.log("err", err);
});

// err Error: 抛出了异常
```

上面这种方式其实是不符合[Promise/A+](https://promisesaplus.com/)规范的（社区制定的规范），其实规范里它规定我们写对应的错误（拒绝）捕获的时候，让我们写在 then 方法的第二个参数里的，但是 ES6 在实现 Promise 的时候，为了让我们代码的阅读性更强，为了让我们编写起来更加方便，给我们提供了 catch 方法

理解了的话我们来看看下面这段代码

```js
const p = new Promise((resolve, reject) => {
  resolve("aaa");
});

p.then((res) => {
  console.log("res", res);
  return new Promise((resolve, reject) => {
    reject("bbb");
  });
}).catch((err) => {
  console.log("err", err);
});

// res aaa
// err aaa
```

那我们再来看这个代码

```js
const p = new Promise((resolve, reject) => {
  resolve("aaa");
});

p.then((res) => {
  console.log("res1", res);
})
  .then((res) => {
    console.log("res2", res);
    throw Error("抛出了异常");
  })
  .then((res) => {
    console.log("res3", res);
  })
  .catch((err) => {
    console.log("err", err);
  });

// res1 aaa
// res2 undefined
// err Error: 抛出了异常
```

### 17.8 Promise 类方法

#### 17.8.1 resolve

前面我们学习的 then、catch、finally 方法都属于 Promise 的实例方法，都是存放在 Promise 的 prototype 上的。

我们有一个对象，希望将其转成 Promise 来使用，以前的话我们可能会这样做

```js
function foo() {
  const obj = {
    name: "tao",
    age: 18,
  };
  return new Promise((resolve, reject) => {
    resolve(obj);
  });
}

console.log(foo()); // Promise { { name: 'tao', age: 18 } }
```

我们其实可以用 Promise.resolve 方
法来完成。

Promise.resolve 的用法相当于 new Promise，并且执行 resolve 操作：

```js
const p = Promise.resolve({ name: "tao", age: 18 });
console.log(p); // Promise { { name: 'tao', age: 18 } }

// 等价于
const p2 = new Promise((resolve) => {
  resolve({ name: "tao", age: 18 });
});

console.log(p2); // Promise { { name: 'tao', age: 18 } }
```

resolve 参数的形态：

- 情况一：参数是一个普通的值或者对象
- 情况二：参数本身是 Promise
- 情况三：参数是一个 thenable

这里就不做演示了，跟前面 resolve 参数是一样的

#### 17.8.2 reject

reject 方法类似于 resolve 方法，只是会将 Promise 对象的状态设置为 reject 状态。

Promise.reject 的用法相当于 new Promise，只是会调用 reject

```js
Promise.reject("tao");
// 等价于
new Promise((reject) => reject("tao"));
```

但是 Promise.reject 传入的参数无论是什么形态，都会直接作为 reject 状态的参数传递到 catch 的。

```js
Promise.reject({
  then() {
    reject("aaa");
  },
})
  .then((res) => {
    console.log("res", res);
  })
  .catch((err) => {
    console.log("err", err);
  });

// err { then: [Function: then] }
```

#### 17.8.3 all

all 是将多个 Promise 包裹在一起形成一个新的 Promise；

新的 Promise 状态由包裹的所有 Promise 共同决定：

- 当所有的 Promise 状态变成 fulfilled 状态时，新的 Promise 状态为 fulfilled，并且会将所有 Promise 的返回值
  组成一个数组；

```js
const p1 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("aaa");
  }, 1000);
});
const p2 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("bbb");
  }, 2000);
});
const p3 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("ccc");
  }, 3000);
});

// 如果传入的值不是Promise，它会帮你把它转为Promise的
// 它会等所有的Promise转为fulfilled的时候，再拿到结果
// 放在数组里是按照顺序的，返回结果
Promise.all([p1, p2, "ccc"]).then((res) => {
  console.log("res", res);
});

// res [ 'aaa', 'bbb', 'ccc' ]
```

当有一个 Promise 状态为 reject 时，新的 Promise 状态为 reject，并且会将第一个 reject 的返回值作为参数；

```js
const p1 = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject("aaa");
  }, 1000);
});
const p2 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("bbb");
  }, 2000);
});
const p3 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("ccc");
  }, 3000);
});

Promise.all([p3, p2, p1])
  .then((res) => {
    console.log("res", res);
  })
  .catch((err) => {
    console.log("err", err);
  });

// err aaa
```

#### 17.8.4 allSettled

all 方法有一个缺陷：当有其中一个 Promise 变成 reject 状态时，新 Promise 就会立即变成对应的 reject 状态。

- 那么对于 resolved 的，以及依然处于 pending 状态的 Promise，我们是获取不到对应的结果的；

在`ES11`（ES2020）中，添加了新的 API Promise.allSettled：

- 该方法会在所有的 Promise 都有结果（settled），无论是 fulfilled，还是 reject 时，才会有最终的状态；
- 并且这个 Promise 的结果一定是 fulfilled 的；

```js
const p1 = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject("aaa");
  }, 1000);
});
const p2 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("bbb");
  }, 2000);
});
const p3 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("ccc");
  }, 3000);
});

Promise.allSettled([p3, p2, p1])
  .then((res) => {
    console.log("res", res);
  })
  .catch((err) => {
    console.log("err", err);
  });

/**
 * res [
  { status: 'fulfilled', value: 'ccc' },
  { status: 'fulfilled', value: 'bbb' },
  { status: 'rejected', reason: 'aaa' }
]
 */
```

allSettled 的结果是一个数组，数组中存放着每一个 Promise 的结果，并且是对应一个对象的；

这个对象中包含 status 状态，以及对应的 value 值；

#### 17.8.5 race

如果有一个 Promise 有了结果，我们就希望决定最终新 Promise 的状态，那么可以使用 race 方法：

race 是竞技、竞赛的意思，表示多个 Promise 相互竞争，谁先有结果，那么就使用谁的结果；

```js
const p1 = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject("aaa");
  }, 1000);
});
const p2 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("bbb");
  }, 2000);
});
const p3 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("ccc");
  }, 3000);
});

Promise.race([p3, p2, p1])
  .then((res) => {
    console.log("res", res);
  })
  .catch((err) => {
    console.log("err", err);
  });

// err aaa
```

#### 17.8.6 any

any 方法是`ES12`中新增的方法，和 race 方法是类似的：

- any 方法会等到一个 fulfilled 状态，才会决定新 Promise 的状态；

```js
const p1 = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject("aaa");
  }, 1000);
});
const p2 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("bbb");
  }, 2000);
});
const p3 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("ccc");
  }, 3000);
});

Promise.any([p3, p2, p1])
  .then((res) => {
    console.log("res", res);
  })
  .catch((err) => {
    console.log("err", err);
  });

// res bbb
```

如果所有的 Promise 都是 reject 的，那么会报一个 AggregateError 的错误。

```js
const p1 = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject("aaa");
  }, 1000);
});
const p2 = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject("bbb");
  }, 2000);
});
const p3 = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject("ccc");
  }, 3000);
});

Promise.any([p3, p2, p1])
  .then((res) => {
    console.log("res", res);
  })
  .catch((err) => {
    console.log("err", err);
    console.log(err.errors);
  });

// err [AggregateError: All promises were rejected]
// [ 'ccc', 'bbb', 'aaa' ]
```

## 十八、迭代器

### 18.1 什么是迭代器

!>迭代器不是 ES6 新增的，只是因为后续讲的生成器是特殊的迭代器（所以迭代器放在了这个专栏）

维基百科：

- `迭代器`（iterator），是确使用户可在容器对象（container，例如链表或数组）上遍访的对象，使用该接口无需关心对象的内部实现细节。
  - 其行为像数据库中的光标，迭代器最早出现在 1974 年设计的 CLU 编程语言中；
  - 在各种编程语言的实现中，迭代器的实现方式各不相同，但是基本都有迭代器，比如 Java、Python 等；

从迭代器的定义我们可以看出来，**迭代器是帮助我们对某个数据结构进行遍历的对象**。

在 JavaScript 中，迭代器也是一个具体的对象，这个对象需要符合`迭代器协议`（iterator protocol）：

- 迭代器协议定义了产生一系列值（无论是有限还是无限个）的标准方式；
- 那么在 js 中这个标准就是一个特定的 next 方法；

next 方法有如下的要求：

- 一个无参数或者一个参数的函数，返回一个应当拥有以下两个属性的对象：
- `done`（boolean）
  - 如果迭代器可以产生序列中的下一个值，则为 false。（这等价于没有指定 done 这个属性。）
  - 如果迭代器已将序列迭代完毕，则为 true。这种情况下，value 是可选的，如果它依然存在，即为迭代结束之后的默认返回值。
- `value`
  - 迭代器返回的任何 JavaScript 值。done 为 true 时可省略。

详情

- https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Iterators_and_generators
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols

前面说过，迭代器在 JS 中表现就是一个实现了特定规则的 next 方法的一个对象

那么我们先来看看什么是迭代器？

我们先不管这段代码是什么意思

这个 itNames 其实就是一个迭代器

```js
const names = ["zs", "ls", "ww", "zl"];

const itNames = names[Symbol.iterator]();

console.log(itNames.next()); // {value: "zs", done: false}
console.log(itNames.next()); // {value: "ls", done: false}
console.log(itNames.next()); // {value: "ww", done: false}
console.log(itNames.next()); // {value: "zl", done: false}
console.log(itNames.next()); // {value: undefined, done: true}
```

其实你会发现,调用迭代器的 next 方法,它会打印一个对象,里面有 done 和 value

我们会发现调用前四次的 value 都一样对应迭代 names 里的值,done 为 false

调用第五次的时候 value 就是 undefined,done 也变成了 true

那我们可以理解当它调用超过 names 的 length 的时候,再调用了话对应的值就变成 undefined,done(完成)就为 true 了

那我们也可以来实现一个迭代器

```js
const names = ["zs", "ls", "ww", "zl"];

// 创建一个迭代器对象来访问数组
// 这个函数返回一个对象，对象中实现了next方法，那么这个函数就相对于是一个创建迭代器对象的函数
function namesIterator(names) {
  let index = 0;
  return {
    next() {
      if (index < names.length) {
        return { done: false, value: names[index++] };
      } else {
        return { done: true, value: undefined };
      }
    },
  };
}

const it = namesIterator(names);
console.log(it.next()); // { done: false, value: 'zs' }
console.log(it.next()); // { done: false, value: 'ls' }
console.log(it.next()); // { done: false, value: 'ww' }
console.log(it.next()); // { done: false, value: 'zl' }
console.log(it.next()); // { done: true, value: undefined }
```

### 18.2 可迭代对象

什么又是可迭代对象呢？

- 它和迭代器是不同的概念；
- 当一个对象实现了`可迭代协议`(iterable protocol)时，它就是一个可迭代对象；
- 这个对象的要求是必须实现 @@iterator 方法，在代码中我们使用 Symbol.iterator 访问该属性；

当我们要问一个问题，我们转成这样的一个东西有什么好处呢？

- 当一个对象变成一个可迭代对象的时候，进行某些迭代操作，比如 for...of 操作时，其实就会调用它的
  @@iterator 方法；

其实迭代器协议和可迭代协议根本不是一个东西

下面是我的个人理解

迭代器协议就是要求实现一个对象实现特定的 next 方法

可迭代协议就是要求实现一个对象实现[Symbol.iterator]属性（函数），函数返回一个迭代器

```js
const names = {
  names: ["zs", "ls", "ww", "zl"],
  [Symbol.iterator]() {
    let index = 0;
    return {
      next: () => {
        if (index < this.names.length) {
          return { done: false, value: this.names[index++] };
        } else {
          return { done: true, value: undefined };
        }
      },
    };
  },
};

const itNames = names[Symbol.iterator]();
console.log(itNames.next());
console.log(itNames.next());
console.log(itNames.next());
console.log(itNames.next());
console.log(itNames.next());

// 其实这就解答了以前困扰我的一个疑惑
// 以前我用for of的时候只知道它是用来遍历数组的
// 其实for...of遍历的东西必须是一个可迭代的对象

for (const item of names) {
  console.log(item); // zs ls ww zl
}
```

### 18.3 原生迭代器对象

事实上我们平时创建的很多原生对象已经实现了可迭代协议，会生成一个迭代器对象的：

- String、Array、Map、Set、arguments 对象、NodeList 集合；

```js
// 1.String
const name = "tao";

for (const item of name) {
  console.log(item); // t a o
}

// 2.Array
const names = ["zs", "ls", "ww", "zl"];
for (const item of names) {
  console.log(item); // zs ls ww zl
}

// 3.Map
// const map = new Map()
// map.set('age', 18);
// map.set("aaa", "bbb");
const map = new Map([
  [{ name: "tao", age: 18 }, "aaa"],
  [{ name: "sandy", age: 21 }, "bbb"],
]);

for (const item of map) {
  console.log(item); // [ { name: 'tao', age: 18 }, 'aaa' ] [ { name: 'sandy', age: 21 }, 'bbb' ]
}

// 4.Set
const set = new Set([20, 30, 40, 20, 30]);
for (const item of set) {
  console.log(item); //  20 30 40
}

// 5.arguments
function foo(x, y, z) {
  for (const item of arguments) {
    console.log(item); // 10 20 30
  }
}
foo(10, 20, 30);
```

### 18.4 可迭代对象的应用

那么这些东西可以被用在哪里呢？

- JavaScript 中语法：for ...of、展开语法（spread syntax）、yield\*（后面讲）、解构赋值（Destructuring_assignment）；
- 创建一些对象时：new Map([Iterable])、new WeakMap([iterable])、new Set([iterable])、new WeakSet([iterable]);
- 一些方法的调用：Promise.all(iterable)、Promise.race(iterable)、Array.from(iterable);

```js
// 1.for...of(前面说过就不展示了)

// 2.展开语法
const info = {
  name: "tao",
  age: 18,
};

const newInfo = { ...info };

// 有个疑问？
// 你info不能进行for...of遍历（你不是一个可迭代对象）
// 但是它可以进行解构
// 这是因为在ES9中新增的特性的（对它进行了特殊的处理），本质上来说按照迭代器的语法是不可以解构的，所以这里它不是用的迭代器

// 3.解构
// 这个也是ES9新增的特性
const { name } = info;

// 4.创建一些其他对象（比如前面讲的Map，Set）

// 5.Promise.all
Promise.all("123").then((res) => {
  console.log(res); // ['1','2','3']
});
```

### 18.5 自定义类的迭代

在前面我们看到 Array、Set、String、Map 等类创建出来的对象都是可迭代对象：

- 在面向对象开发中，我们可以通过 class 定义一个自己的类，这个类可以创建很多的对象：
- 如果我们也希望自己的类创建出来的对象默认是可迭代的，那么在设计类的时候我们就可以添加上
  @@iterator 方法；

案例：创建一个 classroom 的类

- 教室中有自己的位置、名称、当前教室的学生；
- 这个教室可以进来新学生（push）；
- 创建的教室对象是可迭代对象；

```js
class Classroom {
  constructor(name, address, students) {
    this.name = name;
    this.address = address;
    this.students = students;
  }
  entry(newStudnt) {
    this.students.push(newStudnt);
  }

  [Symbol.iterator]() {
    let index = 0;
    return {
      next: () => {
        if (index < this.students.length) {
          return { done: false, value: this.students[index++] };
        } else {
          return { done: true, value: undefined };
        }
      },
    };
  }
}

const room = new Classroom("西虹市小学", "西虹市", [
  "zs",
  "ls",
  "ww",
  "zl",
]);
room.entry("qq");

for (const item of room) {
  console.log(item); // zs ls ww zl qq
}
```

### 18.6 迭代器的中断

迭代器在某些情况下会在没有完全迭代的情况下中断：

- 比如遍历的过程中通过 break、continue、return、throw 中断了循环操作；
- 比如在解构的时候，没有解构所有的值；

那么这个时候我们想要监听中断的话，可以添加 return 方法：

```js
class Classroom {
  constructor(name, address, students) {
    this.name = name;
    this.address = address;
    this.students = students;
  }
  entry(newStudnt) {
    this.students.push(newStudnt);
  }

  [Symbol.iterator]() {
    let index = 0;
    return {
      next: () => {
        if (index < this.students.length) {
          return { done: false, value: this.students[index++] };
        } else {
          return { done: true, value: undefined };
        }
      },
      return() {
        console.log("迭代器提前终止了"); // 迭代器提前终止了
        return { done: true, value: undefined };
      },
    };
  }
}

const room = new Classroom("西虹市小学", "西虹市", [
  "zs",
  "ls",
  "ww",
  "zl",
]);
room.entry("qq");

for (const item of room) {
  console.log(item); // zs ls ww zl
  if (item === "zl") break;
}
```

!>其实监听中断的话用的很少，了解有这个东西就好了

## 十九、生成器

### 19.1 什么是生成器

生成器是 ES6 中新增的一种函数控制、使用的方案，它可以让我们更加灵活的控制函数什么时候继续执行、暂停执
行等

生成器是 ES6 中新增的一种函数控制、使用的方案，它可以让我们更加灵活的控制函数什么时候继续执行、暂停执
行等

`生成器函数也是一个函数，但是和普通的函数有一些区别`：

- 首先，生成器函数需要在 function 的后面加一个符号：\*
- 其次，生成器函数可以通过 yield 关键字来控制函数的执行流程：
- 最后，生成器函数的返回值是一个 Generator（生成器）：
  - 生成器事实上是一种`特殊的迭代器`；
  - MDN：Instead, they return a special type of iterator, called a Generator.

```js
function* foo() {
  console.log("代码执行开始");
  console.log("------------");
  const value1 = 100;
  console.log("第一段代码的结果", value1);
  console.log("------------");
  yield;
  console.log("------------");
  const value2 = 200;
  console.log("第二段代码的结果", value2);
  console.log("------------");
  yield;
  console.log("------------");
  const value3 = 300;
  console.log("第三段代码的结果", value3);
  console.log("------------");
  yield;
  console.log("------------");
  const value4 = 400;
  console.log("第四段代码的结果", value4);
  console.log("------------");
  console.log("代码执行结束");
}
const generator = foo();

console.log(generator.next());
console.log(generator.next());
console.log(generator.next());
console.log(generator.next());

/**
代码执行开始
------------
第一段代码的结果 100
------------
{ value: undefined, done: false }
------------
第二段代码的结果 200
------------
{ value: undefined, done: false }
------------
第三段代码的结果 300
------------
{ value: undefined, done: false }
------------
第四段代码的结果 400
------------
代码执行结束
{ value: undefined, done: true }
*/
```

### 19.2 生成器函数的执行流程

我们发现上面的生成器函数 foo 的执行体压根没有执行，它只是返回了一个生成器对象。

那么我们如何可以让它执行函数中的东西呢？调用 next 即可；

调用第一次 next 方法，就会调用第一个 yield 之前的代码

调用第二次 next 方法，就会调用第一个 yield 到第二个 yield 之间的代码

以此内推。。。。

如果最后只有一个 yield 的话，就会执行 yield 后面的代码，然后使返回的 done 属性变成 true，之后调用 next 方法

理解了前面讲的迭代器的话应该很容易理解这个

下面我们直接通过代码来看一下生成器函数的执行流程是怎么样的

我们会发现打印的 value 值都是 undefined

比如第一个 yield，可以理解为 yield 前面的代码就是一段函数执行体

函数执行体当然也可以返回值咯

默认没有返回，那就是 return undefined

所以我们可以按照以前写函数的逻辑，在最后返回我们想返回的值

```js
function* bar() {
  console.log("代码执行开始");
  console.log("------------");
  const value1 = 100;
  console.log("第一行代码的结果", value1);
  console.log("------------");
  return value1;
  yield;
  console.log("------------");
  const value2 = 200;
  console.log("第二段代码的结果", value2);
  console.log("------------");
  yield;
  console.log("代码执行结束");
}

const generator2 = bar();
console.log(generator2.next());
console.log(generator2.next());

/**
 * 代码执行开始
 *  ------------
 *  第一行代码的结果 100
 *  ------------
 *  { value: 100, done: true }
 *  { value: undefined, done: true }
 */
```

你会发现的确我们可以返回值，但是我们又执行了 next，第二段代码没有符合我们的预期

我们只是想让第一段的代码返回的 value 为 100，第二段代码正常执行，返回 undefined

但是第一段代码返回了以后 done 就为了 true，之前学过迭代器的话应该可以理解

表示这个迭代器执行完毕了，所以之后执行都会返回{done：true，value：undefined}

如果按照以前函数的逻辑的话，显然是不行的，在生成器数中我们可以把 return 理解为终止执行，生成器提前结束
后续我还会详细讲解 return 的

那么我们用什么来返回值喃

在生成器函数中我们会把想返回的值放在 yield 后面（yield 后面就是我们想返回的值）

```js
function* baz() {
  console.log("代码执行开始");
  console.log("------------");
  const value1 = 100;
  console.log("第一行代码的结果", value1);
  console.log("------------");
  yield value1;
  console.log("------------");
  const value2 = 200;
  console.log("第二段代码的结果", value2);
  console.log("------------");
  yield value2;
  console.log("代码执行结束");
}

const generator3 = baz();
console.log(generator3.next());
console.log(generator3.next());
console.log(generator3.next());

/**
代码执行开始
------------
第一行代码的结果 100
------------
{ value: 100, done: false }
------------
第二段代码的结果 200
------------
{ value: 200, done: false }
代码执行结束
{ value: undefined, done: true }
*/
```

### 19.3 生成器的 next 传递参数

函数既然可以暂停来分段执行，那么函数应该是可以传递参数的，我们是否可以给每个分段来传递参数呢？

- 答案是可以的；
- 我们在调用 next 函数的时候，可以给它传递参数，那么这个参数会作为上一个 yield 语句的返回值；
- 注意：也就是说我们是为本次的函数代码块执行提供了一个值；

```js
function* foo(num) {
  console.log("代码执行开始");
  console.log("------------");
  const value1 = 100;
  console.log("第一段代码的结果", value1);
  console.log("------------");
  yield num;
  console.log("------------");
  const value2 = 200;
  console.log(num);
  console.log("第二段代码的结果", value2);
  console.log("------------");
  yield;
  console.log("------------");
  const value3 = 300;
  console.log("第三段代码的结果", value3);
  console.log("------------");
  yield;
  console.log("------------");
  const value4 = 400;
  console.log("第四段代码的结果", value4);
  console.log("------------");
  console.log("代码执行结束");
}

const generator = foo(5);
// 如果我们第一次调用next方法的时候传递参数，可以在foo这里进行传递
console.log(generator.next());
// 那么它其实相当于变成了这样
// function* foo(num) {
//   const num = 5
// }
// foo(5)
// 跟原来的函数是一样的，之后我们可以在函数的任何代码段里获取num的值

/** 
代码执行开始
------------
第一段代码的结果 100
------------
{ value: 5, done: false }
*/
```

第一次调用传递参数的时候 next，我们可能会像上面一样，但是我们想让自己的代码片段有自己的独立的参数值

我们从第二次调用 next 开始，传递的参数值会作为上一次 yield 语句的返回值

这是意思？感觉有点绕啊？

```js
function* bar(num) {
  console.log("代码执行开始");
  console.log("------------");
  const value1 = 100;
  console.log("第一段代码的结果", value1);
  console.log("------------");
  const result = yield num;
  console.log("------------");
  const value2 = 200;
  console.log("第二段代码的结果", value2);
  console.log("------------");
  yield;
  console.log(result);
  console.log("代码执行结束");
}

const generator2 = bar(5);
// result就是我们第二次调用next传递的参数
console.log(generator2.next());
console.log(generator2.next(10));
console.log(generator2.next(10));

/**
代码执行开始
------------
第一段代码的结果 100
------------
{ value: 5, done: false }
------------
第二段代码的结果 200
------------
{ value: undefined, done: false }
10
代码执行结束
{ value: undefined, done: true }
*/
```

### 19.4 生成器的 return 终止执行

还有一个可以给生成器函数传递参数的方法是通过 return 函数：

- return 传值后这个生成器函数就会结束，之后调用 next 不会继续生成值了；

```js
function* foo() {
  console.log("代码执行开始");
  console.log("------------");
  const value1 = 100;
  console.log("第一段代码的结果", value1);
  console.log("------------");
  yield value1;
  console.log("------------");
  const value2 = 200;
  console.log("第二段代码的结果", value2);
  console.log("------------");
  yield value2;
  console.log("------------");
  const value3 = 300;
  console.log("第三段代码的结果", value3);
  console.log("------------");
  yield;
  console.log("------------");
  const value4 = 400;
  console.log("第四段代码的结果", value4);
  console.log("------------");
  console.log("代码执行结束");
}

const generator = foo();

console.log(generator.next());
// return传递的参数会作为此段代码的返回值的value
console.log(generator.return(10));
console.log(generator.next());

/**
代码执行开始
------------
第一段代码的结果 100
------------
{ value: 100, done: false }
{ value: 10, done: true }
{ value: undefined, done: true }
*/
```

### 19.5 生成器的 throw 抛出异常

除了给生成器函数内部传递参数之外，也可以给生成器函数内部抛出异常：

- 抛出异常后我们可以在生成器函数中捕获异常；

```js
function* foo() {
  console.log("代码执行开始");
  console.log("------------");
  const value1 = 100;
  console.log("第一段代码的结果", value1);
  console.log("------------");
  try {
    yield value1;
  } catch (error) {
    console.log("内部捕获到异常", error);
    yield 200;
  }
  console.log("代码执行结束");
}

const generator = foo();

console.log(generator.next());
console.log(generator.throw("抛出异常"));
console.log(generator.next());

/**
代码执行开始
------------ 
第一段代码的结果 100
------------
{ value: 100, done: false }
内部捕获到异常 抛出异常
{ value: 200, done: false }
代码执行结束
{ value: undefined, done: true }
*/
```

### 19.6 生成器替代迭代器使用

我们发现生成器是一种特殊的迭代器，那么在某些情况下我们可以使用生成器来替代迭代器：

```js
function* foo(arr) {
  for (const item of arr) {
    yield item;
  }
}

const names = ["zs", "ls", "ww"];

const it = foo(names);

console.log(it.next());
console.log(it.next());
console.log(it.next());
console.log(it.next());

/**
{ value: 'zs', done: false }
{ value: 'ls', done: false }
{ value: 'ww', done: false }
{ value: undefined, done: true }
*/
```

事实上我们还可以使用`yield*`来生产一个可迭代对象（yield\*后面跟的是一个可迭代的对象）

- 这个相当于是一种 yield 的语法糖，只不过会依次迭代这个可迭代对象，每次迭代其中的一个值；

```js
function* foo(arr) {
  yield* arr;
}

const names = ["zs", "ls", "ww"];

const it = foo(names);

console.log(it.next());
console.log(it.next());
console.log(it.next());
console.log(it.next());

/**
{ value: 'zs', done: false }
{ value: 'ls', done: false }
{ value: 'ww', done: false }
{ value: undefined, done: true }
*/
```

在之前的自定义类迭代中，我们也可以换成生成器：

```js
class Classroom {
  constructor(name, address, students) {
    this.name = name;
    this.address = address;
    this.students = students;
  }
  entry(newStudnt) {
    this.students.push(newStudnt);
  }
  *[Symbol.iterator]() {
    yield* this.students;
  }
}

const room = new Classroom("西虹市小学", "西虹市", [
  "zs",
  "ls",
  "ww",
  "zl",
]);
room.entry("qq");

for (const item of room) {
  console.log(item); // zs ls ww zl qq
}
```

## 二十、异步处理的方案

学完了我们前面的 Promise、生成器等，我们目前来看一下异步代码的最终处理方案。

需求：

- 我们需要向服务器发送网络请求获取数据，一共需要发送三次请求；
- 第二次的请求 url 依赖于第一次的结果；
- 第三次的请求 url 依赖于第二次的结果；
- 依次类推；

### 20.1 回调函数

```js
function requestData(url) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(url);
    }, 1000);
  });
}

function getData() {
  requestData("tao").then((res) => {
    requestData(res + "aaa").then((res1) => {
      requestData(res1 + "bbb").then((res2) => {
        requestData(res2 + "ccc").then((res3) => {
          console.log(res3);
        });
      });
    });
  });
}
getData(); // taoaaabbbccc
```

### 20.2 Promise

```js
function requestData(url) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(url);
    }, 1000);
  });
}

function getData() {
  requestData("tao")
    .then((res) => {
      return requestData(res + "aaa");
    })
    .then((res1) => {
      return requestData(res1 + "bbb");
    })
    .then((res2) => {
      return requestData(res2 + "ccc");
    })
    .then((res3) => {
      console.log(res3);
    });
}
getData(); // taoaaabbbccc
```

### 20.3 Generator

上面的代码其实看起来也是阅读性比较差的，有没有办法可以继续来对上面的代码进行优化呢？

```js
function requestData(url) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(url);
    }, 1000);
  });
}

function* getData(url) {
  const res = yield requestData(url);
  const res1 = yield requestData(res + "aaa");
  const res2 = yield requestData(res1 + "bbb");
  const res3 = yield requestData(res2 + "ccc");
  console.log(res3);
}
const generator = getData("tao");
generator.next().value.then((res) => {
  generator.next(res).value.then((res1) => {
    generator.next(res1).value.then((res2) => {
      generator.next(res2).value.then((res3) => {
        console.log(res3);
      });
    });
  });
});

// taoaaabbbccc
```

那你可能会说，这代码的阅读性不是一样的很差吗

我们发现上面这种方法其实是有规律的，所以，我们可以封装一个工具函数 execGenerator 自动执行生成器函数：

```js
function requestData(url) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(url);
    }, 1000);
  });
}

function* getData(url) {
  const res = yield requestData(url);
  const res1 = yield requestData(res + "aaa");
  const res2 = yield requestData(res1 + "bbb");
  const res3 = yield requestData(res2 + "ccc");
  console.log(res3);
}

function execGenerator(url) {
  const generator = getData(url);
  function exec(res) {
    const result = generator.next(res);
    if (result.done) return result.value;
    result.value.then((res) => {
      exec(res);
    });
  }
  exec();
}

execGenerator("tao"); // taoaaabbbccc
```

其实我们不需要自己来编写 execGenerator 这样的工具函数，有一个叫做[co](https://github.com/tj/co)的包可以帮我们自动执行生成器函数

```js
function requestData(url) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(url);
    }, 1000);
  });
}

function* getData(url) {
  const res = yield requestData(url);
  const res1 = yield requestData(res + "aaa");
  const res2 = yield requestData(res1 + "bbb");
  const res3 = yield requestData(res2 + "ccc");
  console.log(res3);
}

const co = require("co");

// co的第一个参数放入需要执行的生成器函数，第二个参数放入我们想要传入生成器参数的值
co(getData, "tao"); // taoaaabbbccc
```

### 20.4 async/await

但是在`ES8`以后，我们不需要编写上面的代码了，ES8 新增的 async/await 可以帮助我们实现上面的功能

```js
function requestData(url) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(url);
    }, 1000);
  });
}

async function getData(url) {
  const res = await requestData(url);
  const res1 = await requestData(res + "aaa");
  const res2 = await requestData(res1 + "bbb");
  const res3 = await requestData(res2 + "ccc");
  console.log(res3);
}

getData("tao"); // taoaaabbbccc
```

但是你要知道 async/await 其实是 generator 和 promise 实现的一个自动执行的语法糖

## 二十一、模块化

### 21.1 什么是模块化？

到底什么是模块化、模块化开发呢？

- 事实上模块化开发最终的目的是将程序划分成**一个个小的结构**；
- 这个结构中编写属于**自己的逻辑代码**，有**自己的作用域**，不会影响到其他的结构；
- 这个结构可以将自己希望暴露的**变量、函数、对象**等导出给其结构使用；
- 也可以通过某种方式，导入另外结构中的变量、函数、对象等；

上面说提到的**结构**，就是**模块**；按照这种**结构划分**开发程序的过程，就是**模块化开发**的过程；

无论你多么喜欢 JavaScript，以及它现在发展的有多好，它都有很多的缺陷：

- 比如 var 定义的变量作用域问题；
- 比如 JavaScript 的面向对象并不能像常规面向对象语言一样使用 class；
- 比如 JavaScript 没有模块化的问题；

Brendan Eich 本人也多次承认过 JavaScript 设计之初的缺陷，但是随着 JavaScript 的**发展以及标准化**，存在的缺陷问题基
本都得到了完善。

无论是 web、移动端、小程序端、服务器端、桌面应用都被广泛的使用；

### 21.2 模块化的历史

在网页开发的早期，Brendan Eich 开发 JavaScript 仅仅作为一种**脚本语言**，做一些简单的表单验证或动画实现等，那个时
候代码还是很少的：

- 这个时候我们只需要讲 JavaScript 代码写到**script 标签**中即可；
- 并没有必要放到多个文件中来编写；甚至流行：通常来说 JavaScript 程序的**长度只有一行**。

但是随着前端和 JavaScript 的快速发展，JavaScript 代码变得越来越复杂了：

- ajax 的出现，**前后端开发分离**，意味着后端返回数据后，我们需要通过**JavaScript 进行前端页面的渲染**；
- SPA 的出现，前端页面变得更加复杂：包括**前端路由、状态管理**等等一系列复杂的需求需要通过 JavaScript 来实现；
- 包括 Node 的实现，JavaScript 编写**复杂的后端程序**，没有模块化是致命的硬伤；

所以，模块化已经是 JavaScript 一个非常迫切的需求：

- 但是 JavaScript 本身，直到`ES6`（2015）才推出了自己的模块化方案；
- 在此之前，为了让 JavaScript 支持模块化，涌现出了很多不同的模块化规范：**AMD、CMD、CommonJS**等；

### 21.3 没有模块化带来的问题

早期没有模块化带来了很多的问题：比如命名冲突的问题

当然，我们有办法可以解决上面的问题：立即函数调用表达式（IIFE）

但是，我们其实带来了新的问题：

1.  我必须记得**每一个模块中返回对象的命名**，才能在其他模块使用过程中正确的使用；
2.  代码写起来**混乱不堪**，每个文件中的代码都需要**包裹在一个匿名函数中来编写**；
3.  在**没有合适的规范**情况下，每个人、每个公司都可能会任意命名、甚至出现模块名称相同的情况；

所以，我们会发现，虽然实现了模块化，但是我们的实现过于简单，并且是没有规范的。

- 我们需要制定一定的规范来约束每个人都**按照这个规范去编写模块化的代码**；
- 这个规范中应该包括核心功能：**模块本身可以导出暴露的属性，模块又可以导入自己需要的属性**；
- JavaScript 社区为了解决上面的问题，涌现出**一系列好用的规范**，接下来我们就学习具有代表性的一些规范。

### 21.4 CommonJS 规范

我们需要知道 CommonJS 是**一个规范**，最初提出来是在浏览器以外的地方使用，并且当时被命名为**ServerJS**，后来为了
体现它的广泛性，修改为**CommonJS**，平时我们也会**简称为 CJS**。

- Node 是 CommonJS 在服务器端一个具有代表性的实现；
- Browserify 是 CommonJS 在浏览器中的一种实现；
- webpack 打包工具具备对 CommonJS 的支持和转换；

所以，Node 中对**CommonJS 进行了支持和实现**，让我们在开发 node 的过程中可以方便的进行模块化开发：

- 在 Node 中**每一个 js 文件都是一个单独的模块**；
- 这个模块中包括**CommonJS 规范的核心变量**：exports、module.exports、require；
- 我们可以使用这些变量来方便的进行**模块化开发**；

前面我们提到过模块化的核心是导出和导入，Node 中对其进行了实现：

- exports 和 module.exports 可以负责**对模块中的内容进行导出**；
- require 函数可以帮助我们**导入其他模块（自定义模块、系统模块、第三方库模块）中的内容**；

#### 21.4.1 exports 导出

exports 是一个对象，我们可以在这个对象中添加很多个属性，添加的属性会导出

```js
// test.js
const name = "tao";
const age = 18;
function foo() {
  console.log("foo");
}

exports.name = name;
exports.age = age;
exports.foo = foo;
```

另外一个文件中可以导入

```js
// main.js
const test = require("./test");

console.log(test.name);
console.log(test.age);
test.foo();
```

上面这行完成了什么操作呢？理解下面这句话，Node 中的模块化一目了然

- 意味着 main 中的 bar 变量等于 exports 对象；
- 也就是 require 通过各种查找方式，最终找到了 exports 这个对象；
- 并且将这个 exports 对象赋值给了 bar 变量；
- bar 变量就是 exports 对象了；

#### 21.4.2 module.exports

但是 Node 中我们经常导出东西的时候，又是通过 module.exports 导出的：

```js
const name = "tao";
const age = 18;
function foo() {
  console.log("foo");
}

module.exports = {
  name,
  age,
  foo,
};
```

在 Node 中真正用于导出的其实根本不是 exports，而是 module.exports

因为 module 才是导出的真正实现者；

!>内部源码是这样的 module.exports = {},export = modules.export

既然最终导出是通过 module.exports 进行导出的，那 exports 还有什么用？

- CommonJS 规范中是没有 module.exports 的概念的；
- 导出是需要通过 exports 导出的
- 但是 node 在实现的时候最终是用 module.exports 进行导出的

!>既然最终都是通过 module.exports 进行导出的，所以基本上都会使用 module.exports 来进行导出，所以你现在很少会看到通过 exports 来进行导出（有的话也是比较老的项目了）

#### 21.4.3 require

我们现在已经知道，require 是一个函数，可以帮助我们引入一个文件（模块）中导出的对象。

那么，require 的查找规则是怎么样的呢？

详情：https://nodejs.org/dist/latest-v14.x/docs/api/modules.html

这里我总结比较常见的查找规则：导入格式如下：require(X)

- X 是一个 Node 核心模块，比如 path、http
  - 直接返回核心模块，并且停止查找
- X 是以 ./ 或 ../ 或 /（根目录）开头的
  - 将 X 当做一个文件在对应的目录下查找；
    - 如果有后缀名，按照后缀名的格式查找对应的文件
    - 如果没有后缀名，会按照如下顺序：
      - 直接查找文件 X
      - 查找 X.js 文件
      - 查找 X.json 文件
      - 查找 X.node 文件
  - 没有找到对应的文件，将 X 作为一个目录
    - 查找目录下面的 index 文件
    - 查找 X/index.js 文件
    - 查找 X/index.json 文件
    - 查找 X/index.node 文件
  - 如果没有找到，那么报错：not found
- 直接是一个 X（没有路径），并且 X 不是一个核心模块
  - 优先在当前的 node_modules 查找
  - 没有找到的话会往去上一层的 node_modules 查找
  - 直到在根目录上的 node_modules 也没有找到就会报错

#### 21.4.4 模块的加载过程

模块在被第一次引入时，模块中的 js 代码会被运行一次

模块被多次引入时，会缓存，最终只加载（运行）一次

- 为什么只会加载运行一次呢？
- 这是因为每个模块对象 module 都有一个属性：loaded。
- 为 false 表示还没有加载，为 true 表示已经加载；

如果有循环引入，那么加载顺序是什么？

- 如果出现下面模块的引用关系，那么加载顺序是什么呢？
  - 这个其实是一种数据结构：图结构；
  - 图结构在遍历的过程中，有深度优先搜索（DFS, depth first search）和广度优先搜索（BFS, breadth first search）；
  - Node 采用的是深度优先算法：main -> aaa -> ccc -> ddd -> eee ->bbb

![](https://gitee.com/itsandy/picgo-img/raw/master/JavaScript/模块加载图结构.png)

#### 21.4.5 CommonJS 规范缺点

CommonJS 加载模块是同步的：

- 同步的意味着只有**等到对应的模块加载完毕，当前模块中的内容才能被运行**；
- 这个在服务器不会有什么问题，因为服务器**加载的 js 文件都是本地文件**，加载速度非常快；

如果将它应用于浏览器呢？

- 浏览器**加载 js 文件需要先从服务器将文件下载下来**，之后**再加载运行**；
- 那么采用**同步的就意味着后续的 js 代码都无法正常运行**，即使是**一些简单的 DOM 操作**；

所以在浏览器中，我们通常不使用 CommonJS 规范：

- 当然在 webpack 中使用 CommonJS 是另外一回事；
- 因为它会将我们的代码转成浏览器可以直接执行的代码；

在早期为了可以在浏览器中使用模块化，通常会采用 AMD 或 CMD：

- 但是目前一方面现代的浏览器**已经支持 ES Modules**，另一方面借助于 webpack 等工具可以**实现对 CommonJS 或者 ESModule 代码**的转换；

!> AMD 和 CMD 已经使用非常少了，zhe1

### 21.5 AMD 规范

AMD 主要是应用于浏览器的一种模块化规范：

- AMD 是 Asynchronous Module Definition（异步模块定义）的缩写；
- 它采用的是**异步加载模块**；
- 事实上 AMD 的规范还要早于 CommonJS，但是 CommonJS 目前依然在被使用，而 AMD 使用的较少了；

我们提到过，`规范只是定义代码的应该如何去编写`，只有有了具体的实现才能被应用：

- AMD 实现的比较常用的库是[require.js](https://github.com/requirejs/requirejs)和[curl.js](https://github.com/cujojs/curl)；

### 21.6 CMD 规范

CMD 规范也是应用于浏览器的一种模块化规范：

- CMD 是 Common Module Definition（通用模块定义）的缩写；
- 它也采用了异步加载模块，但是它将 CommonJS 的优点吸收了过来；
- 但是目前 CMD 使用也非常少了；

CMD 也有自己比较优秀的实现方案：

- [SeaJS](https://github.com/seajs/seajs)

### 21.7 ES Module

JavaScript 没有模块化一直是**它的痛点**，所以才会产生我们前面学习的社区规范：CommonJS、AMD、CMD 等，
所以在 ES 推出自己的模块化系统时，大家也是兴奋异常。

ES Module 和 CommonJS 的模块化有一些不同之处：

- 一方面它使用了 import 和 export 关键字；
- 另一方面它采用编译期的静态分析，并且也加入了动态引用的方式；

ES Module 模块采用 export 和 import 关键字来实现模块化：

- export 负责将模块内的内容导出；
- import 负责从其他模块导入内容；

!>采用 ES Module 将自动采用严格模式：use strict

#### 21.7.1 案例代码结构组件

这里我在浏览器中演示 ES6 的模块化开发：

```html
<script src="./index.js"></script>
```

```js
import { name, age, foo } from "./foo.js";
console.log(name);
console.log(age);
foo();
```

```js
export const name = "tao";
export const age = 18;
export function foo() {
  console.log("foo");
}
```

我们使用浏览器打开会发现报这样一个错误

Uncaught SyntaxError: Cannot use import statement outside a module

默认情况下我们通过 src 加载的时候，它只会把你当做一个文件来进行加载，如果当做一个模块来加载的话，需要设置 type='module'

```html
<script src="./index.js" type="module"></script>
```

好了以后，我们发现会报这样的错误

Access to script at 'file:///E:/default/index.js' from origin 'null' has been blocked by CORS policy: Cross origin requests are only supported for protocol schemes: http, data, chrome, chrome-extension, chrome-untrusted, https.

这个在 MDN 上面有给出解释：

- https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Modules
- 你需要注意本地测试 — 如果你通过本地加载 Html 文件 (比如一个 file:// 路径的文件), 你将会遇到 CORS 错误，因为
  Javascript 模块安全性需要。
- 你需要通过一个服务器来测试。

我这里使用的 VSCode，VSCode 中有一个插件：**Live Server**

#### 21.7.2 export

export 关键字将一个模块中的变量、函数、类等导出；

我们希望将其他中内容全部导出，它可以有如下的方式：

在语句声明的前面直接加上 export 关键字

```js
export const name = "tao";
export const age = 18;
export function foo() {
  console.log("foo");
}
```

将所有需要导出的标识符，放到 export 后面的 {}中

```js
const name = "tao";
const age = 18;
function foo() {
  console.log("foo");
}

export { name, age, foo };
```

!>这里的 {}里面不是 ES6 的对象字面量的增强写法，{}也不是表示一个对象的,所以 export {name: name}，是错误的写法；

导出时给标识符起一个别名

```js
const name = "tao";
const age = 18;
function foo() {
  console.log("foo");
}

export { name as myName, age as myAge, foo as myFoo };
```

#### 21.7.3 import

import 关键字负责从另外一个模块中导入内容

导入内容的方式也有多种：

import {标识符列表} from '模块'；

```js
import { name, age, foo } from "./foo.js";
```

!>这里的{}也不是一个对象，里面只是存放导入的标识符列表内容；

导入时给标识符起别名

```js
import { name as myName, age as myAge, foo as myFoo } from "./foo.js";
```

通过 \* 将模块功能放到一个模块功能对象（a module object）上

```js
import * as foo from "./foo.js";
```

#### 21.7.4 export 和 import 结合使用

export 和 import 可以结合使用

```js
export * as foo from "./foo.js";
```

为什么要这样做呢？

- 在开发和封装一个功能库时，通常我们希望将暴露的所有接口放到一个文件中；
- 这样方便指定统一的接口规范，也方便阅读；
- 这个时候，我们就可以使用 export 和 import 结合使用；

#### 21.7.5 default

前面我们学习的导出功能都是有名字的导出（named exports）：

- 在导出 export 时指定了名字；
- 在导入 import 时需要知道具体的名字；

还有一种导出叫做默认导出（default export）

- 默认导出 export 时可以不需要指定名字；
- 在导入时不需要使用 {}，并且可以自己来指定名字；
- 它也方便我们和现有的 CommonJS 等规范相互操作；

```js
export default foo;
```

!>在一个模块中，只能有一个默认导出（default export）；

#### 21.7.6 import 函数

通过 import 加载一个模块，是不可以在其放到逻辑代码中的，比如：

```js
if (true) {
  import foo from "./foo.js";
}
// Uncaught SyntaxError: Unexpected identifier
```

为什么会出现这个情况呢？

- 这是因为 ES Module 在被 JS 引擎解析时，就必须知道它的依赖关系；
- 由于这个时候 js 代码没有任何的运行，所以无法在进行类似于 if 判断中根据代码的执行情况；

但是某些情况下，我们确确实实希望动态的来加载某一个模块：

- 如果根据不懂的条件，动态来选择加载模块的路径；
- 这个时候我们需要使用 import() 函数来动态加载；

```js
if (true) {
  // import函数返回的是promise
  import("./foo.js").then((res) => {
    console.log(res);
  });
}
```

#### 21.7.7 import meta

import.meta 是一个给 JavaScript 模块暴露特定上下文的元数据属性的对象。

- 它包含了这个模块的信息，比如说这个模块的 URL；
- 在`ES11`（ES2020）中新增的特性；

```js
console.log(import.meta); // {url: 'http://127.0.0.1:5500/index.js'}
```

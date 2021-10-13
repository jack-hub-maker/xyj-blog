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

那么是不是意味着 foo 变量只有在代码执行阶段才会创建的呢？

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
  { name, age } = { name: "tao", age: 18, friends: ["zs", "ls", "ww"] }
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

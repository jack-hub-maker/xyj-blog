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

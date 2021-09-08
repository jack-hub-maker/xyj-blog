## 为什么需要 this？

在常见的编程语言中，几乎都有 this 这个关键字（Objective-C 中使用的是 self），但是 JavaScript 中的 this 和常见的面向对象语
言中的 this 不太一样：

- 常见面向对象的编程语言中，比如 Java、C++、Swift、Dart 等等一系列语言中，this 通常只会出现在类的方法中。
- 也就是你需要有一个类，类中的方法（特别是实例方法）中，this 代表的是当前调用对象。
- 但是 JavaScript 中的 this 更加灵活，无论是它出现的位置还是它代表的含义。

我们来看一下编写一个 obj 的对象，有 this 和没有 this 的区别

![image.png](https://img14.360buyimg.com/ddimg/jfs/t1/199089/9/7532/173760/61387a4eE9c803d85/31bb17ab8692e482.png)![image.png](https://img14.360buyimg.com/ddimg/jfs/t1/199089/9/7532/173760/61387a4eE9c803d85/31bb17ab8692e482.png)

其实在日常开发中，有 this 还是没有 this 都是可以的，但是当你修改 obj 为 obj2 的时候，下面的 obj 也要改为 obj2，也许这里只有 3 处用到了，万一有 100 处用到了，这样一行一行的修改就太过于冗余了

这里的 this 执行的是当前的对象，所以无论你上面的名字怎么改，下面的 this 都是指向它的，所以这里的 this 就是优化代码

## this 指向什么呢？

我们先说一个最简单的，this 在全局作用于下指向什么？

浏览器中 this 指向的 window（GO）

```js
console.log(this);
console.log(window);
```

![image.png](https://img12.360buyimg.com/ddimg/jfs/t1/58030/38/17276/10487/6138a57eE2b7d3feb/8fa1f9240b1644ac.png)

在 node 中 this 执行的是一个{}

![image.png](https://img12.360buyimg.com/ddimg/jfs/t1/200412/18/7235/3026/6138a5f3E4a9d18b0/d90bdac5dce0118f.png)

这是因为 node 环境执行 js 文件的时候，会把这个文件当做是一个模块，然后会加载这个模块，然后进行编译，最后放入一个函数执行，执行这个函数的时候使用了.call 的方法把函数中的 this 指向为了{}

想了解跟深入的可以看看 node 的源码

但是一般我们都不会在全局中使用 this，一般都是在函数中，接下来我们就来看看函数中 this 的指向

## this 到底指向什么呢？

我们先来看一个让人困惑的问题：

定义一个函数，我们采用三种不同的方式对它进行调用，它产生了三种不同的结果

![image.png](https://img12.360buyimg.com/ddimg/jfs/t1/203370/38/5702/160473/6138a6a6E779aa291/f16be140f5706c18.png)

这个的案例可以给我们什么样的启示呢？

1.  函数在调用时，JavaScript 会默认给 this 绑定一个值；
2.  this 的绑定和定义的位置（编写的位置）没有关系；
3.  this 的绑定和调用方式以及调用的位置有关系；
4.  this 是在运行时被绑定的（动态绑定）；

那么 this 到底是怎么样的绑定规则呢？一起来学习一下吧

1.  默认绑定
2.  隐式绑定
3.  显式绑定
4.  new 绑定

## 默认规则

什么情况下使用默认绑定呢？独立函数调用。

独立的函数调用我们可以理解成函数没有被绑定到某个对象上进行调用；

我们通过几个案例来看一下，常见的默认绑定

```js
// 案例一
function foo() {
  console.log(this);
}

foo();
```

```js
// 案例二
function foo() {
  console.log(this);
}

function bar() {
  foo();
}
function baz() {
  bar();
}

baz();
```

```js
// 案例三
function foo(Fn) {
  Fn();
}

var obj = {
  name: "tao",
  bar: function () {
    console.log(this);
  },
};
foo(obj.bar);
```

```js
// 案例四
function foo() {
  return function () {
    console.log(this);
  };
}
var fn = foo();
fn();
```

这上面所以的案例 this 的打印结果都是 this

## 隐式绑定

另外一种比较常见的调用方式是通过某个对象进行调用的：

也就是它的调用位置中，是通过某个对象发起的函数调用。

obj 对象会被 js 引擎绑定到 fn 函数中的 this 里面

我们通过几个案例来看一下，常见的默认绑定

```js
// 案例一
function foo() {
  console.log(this);
}

var obj = {
  name: "tao",
  foo: foo,
};

obj.foo(); // obj
```

```js
// 案例二
function foo() {
  console.log(this);
}

var bar = {
  name: "bar",
  foo: foo,
};

var bar = bar.foo;
bar(); // window
```

```js
// 案例三
function foo() {
  console.log(this);
}

var bar = {
  name: "bar",
  foo: foo,
};

var baz = {
  name: "baz",
  bar: bar,
};

baz.bar.foo(); // bar
```

## 显示绑定

隐式绑定有一个前提条件：

- 必须在调用的对象内部有一个对函数的引用（比如一个属性）；
- 如果没有这样的引用，在进行调用时，会报找不到该函数的错误；
- 正是通过这个引用，间接的将 this 绑定到了这个对象上；

如果我们不希望在 对象内部 包含这个函数的引用，同时又希望在这个对象上进行强制调用，该怎么做呢？

- JavaScript 所有的函数都可以使用 call 和 apply 方法（这个和 Prototype 有关）。
  - 它们两个的区别这里不再展开；
  - 其实非常简单，第一个参数是相同的，后面的参数，apply 为数组，call 为参数列表（剩余参数）；
- 这两个函数的第一个参数都要求是一个对象，这个对象的作用是什么呢？就是给 this 准备的。
- 在调用这个函数时，会将 this 绑定到这个传入的对象上。

```js
function foo(num1, num2) {
  console.log(num1 + num2, this);
}

foo.apply("aaa");
foo.call("aaa");
```

![image.png](https://img13.360buyimg.com/ddimg/jfs/t1/77982/37/16980/34326/6138b6bdEd87714a3/735293d67f258d00.png)

这样绑定的话，其实是没有什么区别的，但是如果要对这个函数传参的时候，实参的位置就有区别

![image.png](https://img11.360buyimg.com/ddimg/jfs/t1/199446/10/7453/32811/6138b73aE64520d71/7b81947d0769a576.png)

当然还有 bind 可以显式绑定，看下面这段

```js
function foo(num1, num2) {
  console.log(num1 + num2, this);
}

foo.apply("aaa");
foo.apply("aaa");
foo.apply("aaa");
foo.apply("aaa");
foo.apply("aaa");
foo.apply("aaa");
```

![image.png](https://img10.360buyimg.com/ddimg/jfs/t1/198664/26/7461/56910/6138b7a7Ed86aa6ee/844a0086260f8bf4.png)

如果我们希望 foo 的 this 总是显式绑定在 aaa 上，且上面的代码多次编写会显得非常冗余

这个时候就可以 bind 来显式绑定 this

```js
function foo(num1, num2) {
  console.log(num1 + num2, this);
}

var fn = foo.bind("aaa");
fn();
fn();
fn();
```

![image.png](https://img14.360buyimg.com/ddimg/jfs/t1/56469/17/17046/35354/6138b855E13101e2f/64c8a02e34a11757.png)

bind 绑定 this 的时候会返回一个新的函数，之后我们再调用这个函数的时候，这个函数的 this 始终是我们 bind 显式绑定的 this

## new 绑定

JavaScript 中的函数可以当做一个类的构造函数来使用，也就是使用 new 关键字。

使用 new 关键字来调用函数是，会执行如下的操作：

1.  创建一个全新的对象；
2.  这个新对象会被执行 prototype 连接；
3.  这个新对象会绑定到函数调用的 this 上（this 的绑定在这个步骤完成）；
4.  如果函数没有返回其他对象，表达式会返回这个新对象；

```js
function Person(name, age) {
  console.log(this); // this = {}
  this.name = name;
  this.age = age;
  // return this
}
var p = new Person("tao", 20);
console.log(p);
```

其它细节以后面向对象的时候会细谈

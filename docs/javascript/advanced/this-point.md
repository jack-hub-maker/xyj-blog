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

## this 绑定规则

### 默认绑定

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

### 隐式绑定

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

### 显式绑定

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

### new 绑定

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

!> 这里补充一个知识，在 JavaScript 中我们都知道普通的数据类型占据的是 8byte，但是如果你用了很小的 number 的话（其它的基本数据类型我不清楚，深入看的话需要看 v8 的源码），现代 js 引擎会把小的 Number，在 v8 中成为叫做 sim 的东西，然后会对其进行优化，就可能会优化为 4 个字节

### 内置函数的绑定思考

有些时候，我们会调用一些 JavaScript 的内置函数，或者一些第三方库中的内置函数。

- 这些内置函数会要求我们传入另外一个函数；
- 我们自己并不会显示的调用这些函数，而且 JavaScript 内部或者第三方库内部会帮助我们执行；
- 这些函数中的 this 又是如何绑定的呢？

```js
setTimeout(function () {
  console.log(this); // window
}, 2000);
```

其实就是我一个函数和 2000 作为参数传给 setTimeout 这个函数，只是 setTimeout 这个函数没有暴露出来，我们直接可以调用就行了，这里面的 this 为什么是 window，我猜测可能是内部直接调用了 setTimeout，就是前面说的独立函数调用(默认绑定)也就是指向的是 window

```js
var boxEl = document.querySelector(".box");
boxEl.onclick = function () {
  console.log(this); // <div class="box"></div>
};
```

```js
boxEl.addEventListener("click", function () {
  console.log(this);
});
boxEl.addEventListener("click", function () {
  console.log(this);
});
boxEl.addEventListener("click", function () {
  console.log(this);
});
```

这个我猜测内部是通过 boxEl.onClick(boxEl)的方式来隐式绑定了 this，我们都知道通过 onClick 的方式来监听点击事件只能监听一次，但是通过 addEventListener 监听事件就可以多次监听，我觉得可能是内部它在监听的时候把后面传入的函数放入了一个数组里面，然后通过 boxEL.call(boxEl)的方式绑定了 this

```js
var array = [1, 2, 3];
array.forEach(function () {
  console.log(this); // window
});
```

这种高阶函数默认绑定的是 window，但是这种 API 可以传第二个参数，也就是指定 this 指向

![image.png](https://img12.360buyimg.com/ddimg/jfs/t1/200348/6/7461/79884/61397424E42f239f0/c5965ea56872a666.png)

```js
var array = [1, 2, 3];
array.forEach(function () {
  console.log(this);
}, "aaa");
```

![image.png](https://img13.360buyimg.com/ddimg/jfs/t1/61429/4/16577/28088/61397471E85c615fc/187f534eb86ef503.png)

## 规则优先级

学习了四条规则，接下来开发中我们只需要去查找函数的调用应用了哪条规则即可，但是如果一个函数调用位置应用了多
条规则，优先级谁更高呢？

默认规则的优先级最低（毫无疑问，默认规则的优先级是最低的，因为存在其他规则时，就会通过其他规则的方式来绑定 this）

显示绑定优先级高于隐式绑定

![image.png](https://img14.360buyimg.com/ddimg/jfs/t1/198520/21/7610/39159/61397a3dE1ada0924/a93592ae82c4f486.png)

new 绑定优先级高于隐式绑定

![image.png](https://img11.360buyimg.com/ddimg/jfs/t1/49466/25/16913/43937/61397966E8b5d47a5/f53c995068852d12.png)

new 绑定优先级高于 bind

- new 绑定和 call、apply 是不允许同时使用的，所以不存在谁的优先级更高
- new 绑定可以和 bind 一起使用，new 绑定优先级更高

![image.png](https://img11.360buyimg.com/ddimg/jfs/t1/65797/13/16058/38599/613979ebE0ca79b0d/627c15549be95b3c.png)

总结

new 绑定 > 显式绑定(bind/call/apply) > 隐式绑定 > 默认绑定

## this 规则之外

### 忽略显式绑定

我们讲到的规则已经足以应付平时的开发，但是总有一些语法，超出了我们的规则之外。

如果在显示绑定中，我们传入一个 null 或者 undefined，那么这个显示绑定会被忽略，使用默认规则：

![image.png](https://img12.360buyimg.com/ddimg/jfs/t1/71246/7/16740/60700/6139a5fcEed56a7a3/e97bbd29fede9339.png)

### 间接函数引用

另外一种情况，创建一个函数的 间接引用，这种情况使用默认绑定规则。

```js
function foo() {
  console.log(this);
}

var bar = {
  name: "bar",
  foo: foo,
};

var baz = {
  name: "baz",
};

baz.foo = bar.foo;
baz.foo();
```

这段代码我们都知道 foo 的 this 打印为 baz 对象

但是当你这样写的时候就不一样了

![image.png](https://img13.360buyimg.com/ddimg/jfs/t1/55227/24/17415/44645/6139a8dfEd6e98129/16f9a6482d2eb355.png)

这样写的时候 baz.foo 赋值为了一个函数，之后这个函数被直接调用了，那么就是默认绑定

!> 其实这也是可以很好的理解的，但是一般在开发中，我们是不会写这种代码的，主要就是为了面试题（**卷**）

补充一个小知识，我在听老师讲课的时候听老师讲的一个小知识，关于写 JavaScript 代码的时候这个分号加还是不加（规范），其实这个问题社区也有很多在讨论，这个问题其实可以通过 prettier 和 eslint 来进行定制，但是我个人还是建议这个分号能加就加,比如来看下面这段代码

![image.png](https://img13.360buyimg.com/ddimg/jfs/t1/68701/22/16180/32925/6139abd5E01aa2617/d31750c810a2a91a.png)

baz 后面没有加分号，js 引擎在词法分析的时候就会变成这样

```js
var baz = {
  name: "baz",
}((baz.foo = bar.foo))();
```

对象后面如果没有添加分号的话，js 引擎会认为你这段代码还没有结束，之后就会变成上面这个样子

## 箭头函数

箭头函数是 ES6 之后增加的一种编写函数的方法，并且它比函数表达式要更加简洁：

- 箭头函数不会绑定 this、arguments 属性
- 剪头函数不能作为构造函数来使用（不能和 new 一起来使用，会抛出错误）

箭头函数的构造

- () : 参数
- => : 箭头
- {} : 函数执行体

![image.png](https://img14.360buyimg.com/ddimg/jfs/t1/197543/36/7657/23295/6139aee7Eb1887788/794a49b598c71327.png)

高阶函数在使用的时候，也可以传入箭头函数

![image.png](https://img11.360buyimg.com/ddimg/jfs/t1/202685/20/5746/33675/6139af46E7c861fdf/8d67170047cc7c3c.png)

箭头函数也有一些常用的简写：

当函数的参数只有一个的时候，()可以省略

![image.png](https://img12.360buyimg.com/ddimg/jfs/t1/201340/4/6228/16206/6139afc5Ef93a283e/96233ee4fa8f4d63.png)

当函数的执行体只有一行代码的时候，{}可以省略,如果省略了{}，它会默认把这段代码的执行结果作为返回值

![image.png](https://img13.360buyimg.com/ddimg/jfs/t1/197785/34/7526/22632/6139b1ddE2b29d5fd/a0e53810a917ceec.png)

如果你只有一行代码，并且返回一个对象，这个时候可以这样编写

![image.png](https://img13.360buyimg.com/ddimg/jfs/t1/70840/38/17249/20743/6139b2bcEb0da003c/726d68400d1f98d8.png)

### 箭头函数的 this 绑定

箭头函数不使用 this 的四种标准规则（也就是不绑定 this），而是根据外层作用域来决定 this。

我们来看一个模拟网络请求的案例：

![image.png](https://img11.360buyimg.com/ddimg/jfs/t1/80996/23/17395/60307/6139b65aE0167d114/24cf8c5d25c847b6.png)

在没有箭头的函数我们是按照上面这种方法做的

当有了箭头函数以后，我们就可以这样做了

![image.png](https://img12.360buyimg.com/ddimg/jfs/t1/84910/34/18497/55340/6139b6b6E2bf79ef9/0f49274a34bd51b4.png)

关于 this 我这里推荐老师以前写的一篇 this 指向的文章

前端面试之彻底搞懂 this 指向：https://mp.weixin.qq.com/s?__biz=Mzg5MDAzNzkwNA==&mid=2247483847&idx=1&sn=fe8089ded81098b35461d3c14bb85cde&chksm=cfe3f238f8947b2e734221c5131e3a6bc42f2dae66b9640cc0f038e9dffef45dd4a52d8dd930&scene=178&cur_album_id=1566035091556974596#rd

### 箭头函数没有显示原型

箭头函数是没有显式原型的，所以不能作为构造函数，使用 new 来创建对象；

不了解构造函数和 new（可以去面向对象专栏了解）

```js
const foo = () => {
  console.log(foo.prototype); // undefined
};
foo();

const p = new foo(); // TypeError: foo is not a constructor
```

# JavaScript 的执行过程

假如我们有下面一段代码，它在 JavaScript 中是如何被执行的呢？

```js
var age = 19;
var num1 = 20;
var num2 = 30;
console.log(result);
var result = num1 + num2;
```

后面我会从 js 引擎和内存的角度来看 JavaScript 的执行过程，我也非常喜欢一句话（当你理解了内存，很多东西你都会理解）

## 一、初始化全局对象

js 引擎会在执行代码之前，会在堆内存中创建一个全局对象：Global Object（GO）

- 该对象 所有的作用域（scope）都可以访问；
- 里面会包含 Date、Array、String、Number、setTimeout、setInterval 等等；
- 其中还有一个 window 属性指向自己；
- 这就解释了为什么我们可以打印 window.window.window

## 二、执行上下文栈（调用栈）

js 引擎内部有一个执行上下文栈（Execution Context Stack，简称 ECS），它是用于执行代码的调用栈。

那么现在它要执行谁呢？执行的是全局的代码块：

- 全局的代码块为了执行会构建一个 Global Execution Context（GEC）；
- GEC 会 被放入到 ECS 中 执行；

GEC 被放入到 ECS 中里面包含两部分内容：

1.  在代码执行前(编译阶段)，在 parser 转成 AST 的过程中，会将全局定义的变量、函数等加入到 GlobalObject 中，
    但是并不会赋值(也就是说值为 undefined) - 这个过程也称之为变量的作用域提升（hoisting）
2.  在代码执行中，对变量赋值，或者执行其他的函数；

GEC 全局执行上下文中有一个 variable object 简称 VO（变量对象），这个 VO 指向的其实就是全局对象 GO，之后编译完毕就会开始执行全局代码了

![image.png](https://img12.360buyimg.com/ddimg/jfs/t1/63957/37/17038/42609/613707beEc834081e/0af476cdbe082cb6.png)

## 三、GEC 开始执行代码

```js
var age = 19;
var num1 = 20;
var num2 = 30;
console.log(result);
var result = num1 + num2;
```

都知道这里的 result = undefined，也知道是作用域提升的原理，那为什么会提升，可能不是很清楚，接下来我们就来实际看看这段代码是如何进行执行的

console.log 这种代码是代码真正执行的时候才会执行

![image.png](https://img10.360buyimg.com/ddimg/jfs/t1/47131/10/16885/60924/6137099dE08ffc214/37e4d4f5e906fdb4.png)

那接下来代码就会真正开始执行了

代码是从上到下一行一行开始执行的，也就是执行的时候 age 从 undefined 变成了 19，num1 从 undefined 变成了 20，num2 从 undefined 变成了 30，打印 result 变量，GO 中有 result 是 undefined，所以打印出来的 result 是 undefined，然后 result 赋值为了 20+30 = 50

![image.png](https://img13.360buyimg.com/ddimg/jfs/t1/6551/33/11833/62517/61370b1fE72944d99/2f57298a1475dab3.png)

## 四、遇到函数如何执行？

在执行的过程中执行到一个函数时，就会根据函数体创建一个函数执行上下文（Functional Execution Context，
简称 FEC），并且压入到 EC Stack 中。

FEC 中包含三部分内容：

1.  在解析函数成为 AST 树结构时，会创建一个 Activation Object（AO）：AO 中包含形参、arguments、函数定义和指向函数对象、定义的变量；
2.  作用域链：由 VO（在函数中就是 AO 对象）和父级 VO 组成，查找时会一层层查找；
3.  this 绑定的值：这个我们后续会详细解析；

当我们创建了一个函数的时候，堆内存中就会创建一个函数对象来存储，函数对象中存储着很多东西，这里我就选两个比较重要的

- 一个叫做 parent scope（父级作用域）
- 一个是函数的执行体

执行函数前也会有一个解析：VO 指向的是一个叫做 AO 的活跃对象，那么真正执行函数前会在堆内存就会创建一个叫做当前函数的 AO 对象，然后会解析函数内的代码

听起来可能有点懵，但画图出来就很只直观了

比如下面这段代码

```js
var age = 21;
function foo() {
  var age = 19;
  console.log(age);
}
foo();
```

我还是画两个图方便理解

![image.png](https://img11.360buyimg.com/ddimg/jfs/t1/204718/7/5361/57581/613711f6Eb80c1c38/defc9d10384adb1f.png)

![image.png](https://img10.360buyimg.com/ddimg/jfs/t1/53233/15/16768/87760/61371385Ec112d2bd/fee2191b67811976.png)

当执行函数的时候遇到对变量进行操作的时候，这个时候函数 VO 中会有一个作用域链：当前 VO+parent scope 父级作用域进行查找，如果父级作用域也没有找到，就会一层一层往上找，直到在全局 GO 中还没有找到就会报错

当函数执行完毕之后，函数执行上下文 FEC 会弹出栈，也就是销毁了，那么函数的 VO 指向的 AO 这条线是不是也应该没有了

![image.png](https://img12.360buyimg.com/ddimg/jfs/t1/55726/5/17179/70310/6137151fE824b4287/f8d444b84efb5184.png)

其实堆内存中的 FOO AO 也会销毁（JavaScript 回收器会帮我们进行回收掉，后面会讲）

## 五、变量环境和记录

其实我们上面的讲解都是基于早期 ECMA 的版本规范：

![image.png](https://img11.360buyimg.com/ddimg/jfs/t1/200777/3/5941/104681/613715aeEdf56d4bd/fc2104b484e52082.png)

在最新的 ECMA 的版本规范中，对于一些词汇进行了修改：

![image.png](https://img12.360buyimg.com/ddimg/jfs/t1/198640/4/7114/137868/613715ceEb9c30863/659acb2a5d32ce7c.png)

通过上面的变化我们可以知道，在最新的 ECMA 标准中，我们前面的变量对象 VO 已经有另外一个称呼了变量环境
VE。


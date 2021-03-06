# 闭包

## 一、让人迷惑的闭包

闭包是 JavaScript 中一个非常容易让人迷惑的知识点：

这里有一段《你不知道的 JavaScript（上）》作者写的一段话

![image.png](https://img11.360buyimg.com/ddimg/jfs/t1/78783/10/16944/233522/6136112eE4274bc84/20f65d7a9588efec.png)

闭包确实是 JavaScript 中一个很难理解的知识点，接下来我们就对其一步步来进行剖析，看看它到底有什么神奇之
处。

## 二、JS 中闭包的定义

这里先来看一下闭包的定义，分成两个：在计算机科学中和在 JavaScript 中。

在计算机科学中对闭包的定义（维基百科）：

- 闭包（英语：Closure），又称**词法闭包**（Lexical Closure）或**函数闭包**（function closures）；
- 是在支持 **头等函数** 的编程语言中，实现词法绑定的一种技术；
- 闭包在实现上是一个结构体，它存储了一个函数和一个关联的环境（相当于一个符号查找表）；
- 闭包跟函数最大的区别在于，当捕捉闭包的时候，它的 **自由变量** 会在捕捉时被确定，这样即使脱离了捕捉时的上下文，它也能照常运行；

闭包的概念出现于 60 年代，最早实现闭包的程序是 Scheme，那么我们就可以理解为什么 JavaScript 中有闭包：

因为 JavaScript 中有大量的设计是来源于 Scheme 的；

我们再来看一下 MDN 对 JavaScript 闭包的解释：

- 一个函数和对其周围状态（lexical environment，**词法环境**）的引用捆绑在一起（或者说函数被引用包围），这样的组合就是闭包（closure）；
- 也就是说，闭包让你可以在一个内层函数中访问到其外层函数的作用域；
- 在 JavaScript 中，每当创建一个函数，闭包就会在函数创建的同时被创建出来；

个人理解

?>闭包 = 函数 + 自由变量

- 一个普通的函数 function，如果它可以访问外层作用域的自由变量，那么这个函数就是一个闭包；
- 从广义的角度来说：JavaScript 中的函数都是闭包；
- 从狭义的角度来说：JavaScript 中一个函数，如果访问了外层作用域的变量，那么它是一个闭包；

!> 看上面的话可能会有点懵，不急先看看后面通过内存的角度来解析闭包，之后再回来你就可以理解上面的话了

## 四、闭包的访问过程

如果我们编写了如下的代码，它一定是形成了闭包的：

```js
var age = 19;
function foo() {
  function bar() {
    console.log(age);
  }
  return bar;
}
var fn = foo();
fn();
```

因为我们前面说过嘛，一个函数访问了外层作用域的变量，那么它就是一个闭包

接下来我们通过内存图来看看这个函数是如何访问的

学习了前面的知识，应该对内存的概念是比较清晰了，我就直接放出我自己画的内存图

![image.png](https://img10.360buyimg.com/ddimg/jfs/t1/204844/34/5150/77871/6136c6c8Eef5b8e97/997fc815a8a10178.png)

正常情况下 foo 执行完毕，foo 就会被弹出栈，也就意味 FOO 的 VO 没有指向堆内存的 AO 了，那我们的 foo AO 是不是也应该要被销毁

但是 FOO AO 并没有被销毁，这是为什么

前面我们不是讲过 JS 的内存管理吗，GC 算法的标记算法会从根对象开始（也就是 GO），相对于从 GO 开始连线，没有连到的就会被 GC 回收掉

根据内存图可知 Foo AO 在 GO 那条线上，所以不会被 GC 回收掉

## 五、闭包的内存泄露

闭包解决了什么问题

- 避免污染全局环境(因为使用的是局部变量)
- 提供对局部变量的间接访问(age 只能+1,不能-1)
- 维持变量,使其不会被垃圾回收

我们可能经常会听到闭包**可能**会造成内存泄漏

正确的说法是: 闭包的**使用不当**会造成内存泄漏

举例说明:

```js
let outer = function () {
  let name = "Jake";
  // 假如这里有1000行代码
  return function () {
    return name;
  };
};
```

在上面的案例中，如果 outer 只用一次，那么我们是不是想让这个 outer 被垃圾回收掉

但是因为 GC 算法无法对它进行回收

因为我们不用它了，但是它依旧存储在内存中，久而久之就可能造成内存泄漏

所以我们经常会听到闭包会造成内存泄漏

1. 就从上面的例子来看，如果我们不会使用 name 以及后面的说的 1000 行代码来看，因为这些地方的内存是不会被释放的，不再需要的内存使用完后肯定需要释放掉，否则这块内存就浪费了，相对于内存泄漏了
2. 但是如果这段代码我们后续还会继续使用的话，那当然就没有内存泄漏的说法咯

## 六、AO 不使用的属性

我们来研究一个问题：AO 对象不会被回收时，是否里面的所有属性都不会被释放？

```js
function foo() {
  var name = "tao";
  var age = 19;
  function bar() {
    console.log(name);
  }
  return bar;
}
var fn = foo();
fn();
```

我们看上面这段代码中 name 属于闭包的父级作用域的变量

我们知道形成闭包之后 foo 函数的 ao 对象是不会被回收的

但是我们思考一个问题？

bar 函数始终不会用到 age 变量，意味着我们想让 age 进行回收掉啊

从 js 引擎的角度上来说 fooAO 对象是不会被回收的

但是在 V8 中 age 是会被回收掉的，为什么我们常说 v8 性能高，v8 也在很多细节上面做了很多的处理，就比如这里的 age

怎么验证我的说法喃？

我们可以在 console.log 的上一行打上一个 debugger，意味着执行了这一行的时候就停止了

然后我们打开开发者工具的 Sources 里看一下

![image.png](https://img11.360buyimg.com/ddimg/jfs/t1/205790/18/5414/36482/613868b1Ef842dd78/58d978c49f341f52.png)

既然我们的程序停在了 bar 函数里面，那么我们也可以在控制台里面打印 name 和 age

![image.png](https://img10.360buyimg.com/ddimg/jfs/t1/64984/24/17329/9670/61386925Ee4ada91e/383271bf4f3b6528.png)

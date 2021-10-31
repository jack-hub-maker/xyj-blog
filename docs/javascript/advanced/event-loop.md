# 事件循环


## 一、进程和线程

线程和进程是操作系统中的两个概念：
- **进程**（process）：计算机**已经运行的程序**，是**操作系统管理程序**的一种方式；
- **线程**（thread）：操作系统能够运行**运算调度的最小单位**，通常情况下**它被包含在进程**中；

听起来很抽象，这里还是给出我的解释：
- 进程：我们可以认为，启动一个应用程序，就会默认启动一个进程（也可能是多个进程）；
- 线程：每一个进程中，都会启动至少一个线程用来执行程序中的代码，这个线程被称之为**主线程**；
- 所以我们也可以说进程是线程的`容器`；

再用一个形象的例子解释：
- 操作系统类似于一个大工厂；
- 工厂中里有很多车间，这个车间就是进程；
- 每个车间可能有一个以上的工人在工厂，这个工人就是线程；


![](https://gitee.com/itsandy/picgo-img/raw/master/JavaScript/进程线程.png)

## 二、操作系统的工作方式

操作系统是如何做到同时让多个进程（边听歌、边写代码、边查阅资料）同时工作呢？
- 这是因为**CPU的运算速度非常快**，它可以**快速的在多个进程之间迅速的切换**；
- 当我们**进程中的线程**获取到**时间片**时，就可以**快速执行我们编写的代码**；
- 对于用户来说**是感受不到这种快速的切换**的；

你可以在Mac的活动监视器或者Windows的资源管理器中查看到很多进程：

![](https://gitee.com/itsandy/picgo-img/raw/master/JavaScript/任务管理器.png)

## 三、浏览器中的JavaScript线程

我们经常会说JavaScript是单线程的，但是JavaScript的线程应该有自己的容器进程：**浏览器或者Node**。

浏览器是一个进程吗，它里面只有一个线程吗？
- 目前**多数的浏览器其实都是多进程的**，当我们**打开一个tab页面时就会开启一个新的进程**，这是为了**防止一个页面卡死而造成所有页面无法响应**，整个浏览器需要强制退出；
- **每个进程中又有很多的线程**，其中**包括执行JavaScript代码的线程**；

JavaScript的代码执行是在一个单独的线程中执行的：
- 这就意味着JavaScript的代码，在**同一个时刻只能做一件事**；
- 如果**这件事是非常耗时**的，就意味着当前的线程就**会被阻塞**；

所以真正耗时的操作，实际上并不是由JavaScript线程在执行的：
- 浏览器的每个进程是多线程的，那么**其他线程可以来完成这个耗时的操作**；
- 比如**网络请求、定时器**，我们只需要在特性的时候执行应该有的回调即可；


## 四、浏览器的事件循环

如果在执行JavaScript代码的过程中，有异步操作呢？
- 比如setTimeout的函数调用
- 这个函数被放到入调用栈中，执行会立即结束，并不会阻塞后续代码的执行；

![](https://gitee.com/itsandy/picgo-img/raw/master/JavaScript/浏览器的事件循环.png)

## 五、宏任务和微任务

但是事件循环中并非只维护着一个队列，事实上是有两个队列：
- **宏任务队列**（macrotask queue）：ajax、setTimeout、setInterval、DOM监听、UI Rendering等
- **微任务队列**（microtask queue）：Promise的then回调、 Mutation Observer API、queueMicrotask()等

那么事件循环对于两个队列的优先级是怎么样的呢？
- **main script中的代码优先执行**（编写的顶层script代码）；
- 在**执行任何一个宏任务之前（不是队列，是一个宏任务）**，都会**先查看微任务队列中是否有任务需要执行**
  - 也就是宏任务执行之前，必须保证微任务队列是空的；
  - 如果不为空，那么就优先执行微任务队列中的任务（回调）；

## 六、Node的事件循环

浏览器中的EventLoop是根据HTML5定义的规范来实现的，不同的浏览器可能会有不同的实现，而Node中是由
libuv实现的。

这里我们来给出一个Node的架构图：
- 我们会发现libuv中主要维护了一个EventLoop和worker threads（线程池）；
- EventLoop负责调用系统的一些其他操作：文件的IO、Network、child-processes等

libuv是一个多平台的专注于异步IO的库，它最初是为Node开发的，但是现在也被使用到Luvit、Julia、pyuv等其
他地方；

![](https://gitee.com/itsandy/picgo-img/raw/master/JavaScript/node架构.png)

## 七、Node事件循环的阶段

我们最前面就强调过，`事件循环像是一个桥梁`，是连接着应用程序的**JavaScript和系统调用**之间的通道：
- 无论是我们的文件IO、数据库、网络IO、定时器、子进程，在完成对应的操作后，都会将**对应的结果和回调函数**放到事件循环（任务队列）中；
- 事件循环会不断的从**任务队列中取出对应的事件（回调函数）**来执行；

但是一次完整的事件循环Tick分成很多个阶段：
- 定时器（Timers）：本阶段执行已经被 setTimeout() 和 setInterval() 的调度回调函数。
- 待定回调（Pending Callback）：对某些系统操作（如TCP错误类型）执行回调，比如TCP连接时接收到
ECONNREFUSED。
- idle, prepare：仅系统内部使用。
- 轮询（Poll）：检索新的 I/O 事件；执行与 I/O 相关的回调；
- 检测（check）：setImmediate() 回调函数在这里执行。
- 关闭的回调函数：一些关闭的回调函数，如：socket.on('close', ...)。

![](https://gitee.com/itsandy/picgo-img/raw/master/JavaScript/node事件循环图.png)
## 八、Node的宏任务和微任务

我们会发现从一次事件循环的Tick来说，Node的事件循环更复杂，它也分为微任务和宏任务：
- 宏任务（macrotask）：setTimeout、setInterval、IO事件、setImmediate、close事件；
- 微任务（microtask）：Promise的then回调、process.nextTick、queueMicrotask；

但是，Node中的事件循环不只是 微任务队列和 宏任务队列：

在微任务和宏任务中也有不同的优先级

微任务队列（按着优先级排序）
- next tick queue：process.nextTick；
- other queue：Promise的then回调、queueMicrotask；

宏任务队列（按着优先级排序）
- timer queue：setTimeout、setInterval；
- poll queue：IO事件；
- check queue：setImmediate；
- close queue：close事件；

!>听懂了，来做几道[面试题](interview/javascript/event-loop?id=事件循环面试题)吧
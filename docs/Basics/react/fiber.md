笔墨藏久，重拾旧装，继续前行。断更了很久，想着拿起笔写点东西，想了半天不知道该怎么写，感觉可能还是有些地方理解的不够到位，就当是为了记录一下学习的过程，硬着头皮写了一些拙见...

# Fiber是什么  
`Fiber` 是 `React` 中一种用于实现虚拟 `DOM` 和组件协调的新的架构。它是 `React 16` 中引入的重要概念，旨在优化渲染过程、实现异步渲染，并提高应用的性能和用户体验。  

`Fiber` 也称协程,它和线程不一样,协程本身是没有并发和并行能力的,它需要配合线程,它只是一种控制流程让出机制。很多时候我们称他为Fiber树，这是因为Fiber既可以看作是链表,也可以看作是树。说他是树,是因为它的形状像树,但是并没有树的特征,树是一个只有指向子节点的指针,并没有指向父节点的指针。  
  
> tips:线程是并发执行的基本单位，一个进程可以包含多个线程，每个线程独立执行。而协程则是在单个线程中实现的并发。协程通过协作式的方式在同一个线程中切换执行，可以避免多线程之间的竞争条件和锁的使用，减少了线程之间的切换开销和资源消耗。  
  
其实协程和线程并不一样，协程本身是没有并发或者并行能力的（需要配合线程），因为它只是一种控制流程的让出机制。JavaScript 提供了一种称为生成器 **Generator** 的功能，它可以用于实现协程。生成器函数可以暂停执行，并且在需要的时候可以从上一个暂停的位置恢复执行。通过调用生成器的 **next()** 方法，可以逐步执行生成器函数的代码并获取生成的值。此外，还可以使用 yield 关键字来指定生成器函数的暂停点，并将值传递给调用者。  
为了更好的理解协程，可以和普通函数一起来看, 以Generator为例:  
  
普通函数执行的过程中无法**被中断和恢复  
  
```  
const tasks = []   
function run() {   
let task   
while (task = tasks.shift()) {   
execute(task)  
}   
}  
```  
再来看看 `Generator` :  
```  
// 任务列表  
const tasks = []  
function * run () {  
let task  
while(task = task.shift()) {  
// 如果有高优先级的任务  
if (hasHighPriorityTask()) {  
// 中断  
yield  
}  
}  
}  
// 中断后恢复  
const iterator = run()  
// 这样就能恢复了  
iterator.next()  
```  
React Fiber 的思想和协程的概念是契合的: **React 渲染的过程可以被中断，可以将控制权交回浏览器，让位给高优先级的任务，浏览器空闲后再恢复渲染**。

# Fiber的结构
`Fiber` 还可以理解为是一种数据结构，前面说了`React Fiber` 其实就是一个链表结构。每个 `Virtual DOM` 都可以表示为一个 `fiber`，如下图所示:
![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3dad1a8edfb44f24871cd12b1646fec8~tplv-k3u1fbpfcp-watermark.image?)
```
/**
 * fiber架构
 * type: 标记类型
 * key: 标记当前层级下的唯一性
 * child : 第一个子元素 fiber
 * sibling ： 下一个兄弟元素 fiber
 * return： 父fiber
 * node： 真实dom节点
 * props：属性值
 * base: 上次的节点 fiber
 * effectTag: 标记要执行的操作类型（删除、插入、更新）
 */
```
上面是一个正常的fiber架构里常见的几个属性，除了一些继承下来的属性，下面说几个关键点：

key : 唯一标识fiber节点的属性，帮助react识别哪些节点可以被添加移除或重排

sibling : 下一个兄弟元素 fiber，类似于链表结构，按同级元素一个链接一个，实现同级元素的快速插入、删除  

child : 第一个子元素的 fiber，支持向下递归。每个Fiber节点可以有零个或多个子节点，这些子节点是当前节点的直接下级。子节点的处理顺序可以根据算法进行调整，以优化渲染性能。

return : 父 fiber。

每个 `fiber` 树它可以由多个子 `fiber` 组成，在首次渲染的时候，会创建 `fiberRoot` 和 `rootFiber`,`fiberRoot` 是整个应用的根节点,`rootFiber` 是组件的根节点,这也就是为什么要求你的组件或者页面的父元素必须是一个单节点。其主要为了方便React进行组件的协调和渲染。如果根元素的直接子元素不止一个节点，React无法确切确定哪个子元素是根节点，从而导致无法正常渲染和更新。

在 `React` 的 `fiber` 中多次更新最多会存在两棵 `Fiber` 树，显示在屏幕上的叫做 `current Fiber` 树，正在内存构建的是 `workInProgress Fiber` 树。

当React开始处理更新时，它首先会复制一份当前的Fiber树，这份复制的树就是`workInProgress Fiber`树。React会在`workInProgress Fiber`上进行所有的协调和变更，而不是直接在`current Fiber`上进行操作。这样做的好处是，在处理更新期间，React可以中断和恢复渲染过程，使得应用可以保持响应性。

当所有的协调和变更完成后，React会对比`current Fiber`和`workInProgress Fiber`之间的差异，以确定需要更新的部分。然后，React将这些更新应用于DOM，将`workInProgress Fiber`树变为新的`current Fiber`树。这个过程被称为"提交"。

通过引入`workInProgress Fiber`树，React能够实现渐进式的渲染。这意味着React可以将渲染工作分成多个步骤，在每个步骤之间进行调度和中断。这有助于提高应用的性能和用户体验，并允许处理大型、复杂的组件树而不会导致阻塞或掉帧。

总结起来就是，`current Fiber`树是当前显示在屏幕上的Fiber树，而`workInProgress Fiber`树是正在内存构建和协调的Fiber树。它们的存在使得React能够更好地处理更新，并实现渐进式的渲染。

来点图示，比如在构建的过程中,`fiberRoot.current` 指向当前界面对应的 `fiber` 树:

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3bad165a7dc94147b8b2f30aa2eff989~tplv-k3u1fbpfcp-watermark.image?)

构造完成并渲染, 切换 `fiberRoot.current` 指针, 使其继续指向当前界面对应的 `fiber` 树:

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ba4ffb509a8d4896bb350b7946e76c43~tplv-k3u1fbpfcp-watermark.image?)
整个过程使用深度遍历的方法,对比新老节点,它类似于 `diff` 算法,判断是否发生了变化以及采取相对应的措施,复用还是变更。

# React-reconciler概念
感觉还是得先提出一下这玩意，这个东西是react 得以运行的核心包(综合协调`react-dom`,`react`,`scheduler`各包之间的调用与配合).  
管理 react 应用状态的输入和结果的输出. 将输入信号最终转换成输出信号传递给渲染器.

具体的流程大概是这样：
-   接受输入(`scheduleUpdateOnFiber`), 将`fiber`树生成逻辑封装到一个回调函数中(涉及`fiber`树形结构, `fiber.updateQueue`队列, 调和算法等),
-   然后把这个回调函数(`performSyncWorkOnRoot`或`performConcurrentWorkOnRoot`)送入`scheduler`进行调度
-   `scheduler`会控制回调函数执行的时机, 回调函数执行完成后得到全新的 fiber 树
-   再调用渲染器(如`react-dom`, `react-native`等)将 fiber 树形结构最终反映到界面上

# scheduler概念
这个东西可以说是fiber里面或者说是react里面最核心的东西之一了，是调度机制的核心实现, 控制由`react-reconciler`送入的回调函数的执行时机, 在`concurrent`模式下可以实现任务分片。

大概的流程如下：
-   核心任务就是执行回调(回调函数由`react-reconciler`提供)
-   通过控制回调函数的执行时机, 来达到任务分片的目的, 实现可中断渲染(`concurrent`模式下才有此特性)

# Concurrent模式
什么是`Concurrent`模式？

这是一个新的特性集合或者说是新的渲染模式。他可以让你的 `React` 应用保持响应,可以根据用户的设备能力和网络情况优雅地调整。随着 `React18` 版本的发布, `Concurrent` 模式成为了 `React` 的默认模式。它主要分为两个方向的优化,它分别是 `CPU密集型` 和 `I/O密集型`。

1.  CPU密集型优化：CPU密集型任务是指那些需要进行大量计算或复杂逻辑处理的任务。在传统的同步渲染模式下，当有一个CPU密集型任务在进行时，React会阻塞主线程，导致页面无响应。而在Concurrent模式下，React通过将渲染工作拆分成小的任务单元，并利用浏览器的空闲时间来执行这些任务单元，从而实现任务的并发执行。这样，即使有CPU密集型任务进行，React可以将其任务分散到多个渲染帧中执行，保证了页面的响应性，避免了阻塞。
1.  I/O密集型优化：I/O密集型任务是指那些需要进行大量异步操作或者I/O操作（如网络请求、读写文件等）的任务。在传统的同步渲染模式下，进行I/O操作时，React会等待这些操作完成后再执行渲染工作。这会导致页面卡顿，用户体验下降。而在Concurrent模式下，React可以利用调度器和调度优先级的机制，根据I/O操作的优先级，决定任务的执行顺序。高优先级的I/O任务会被优先执行，这样可以保证对用户交互和数据的快速响应。同时，React还可以利用浏览器的异步API（如requestIdleCallback）来异步执行这些I/O任务，并且通过将多个更新操作合并为一个更新批次，减少对实际DOM的操作次数，从而进一步提升渲染性能。

`CPU密集型优化`：让位给高优先级

`I/O密集型优化`：异步渲染+批量更新

总结一下这两个方向的优化就是：更好地管理和调度渲染任务，以适应复杂场景和性能要求较高的应用，同时提高了应用的性能和用户体验。

# Concurrent 模式给我们带了什么?
1.  **更好的用户体验**：Concurrent 模式使得 React 应用能够更好地响应用户的交互。通过并发渲染，React 可以以更快的速度进行初始渲染，减少首屏加载时间。同时，它还可以将渲染任务分成多个小任务，保持应用的响应性，避免阻塞用户界面。
1.  **更高的性能**：由于并发渲染的特点，React 可以更高效地利用 CPU 资源。它可以根据任务的优先级和时间片来动态分配渲染工作，从而提升应用的渲染性能。特别是在处理大型和复杂的应用时，Concurrent 模式能够更有效地渲染组件。
1.  **更好的代码可维护性**：Concurrent 模式引入了新的组件生命周期函数(`useDeferredValue`、`useTransition`等)，这些函数可以帮助我们优化组件的渲染逻辑和状态管理。通过使用这些新的特性，我们可以更好地组织和管理 React 组件的代码，提高代码的可维护性和可读性。
1.  **更强大的异步渲染能力**：Concurrent 模式为 React 提供了更强大的异步渲染能力。通过使用新的异步渲染 API，我们可以更细粒度地控制组件的渲染顺序和优先级。这使得我们能够在处理大量数据或计算密集型任务时，更好地管理渲染的过程。

总结一下，Concurrent 模式为React应用带来了更好的用户体验、更高的性能、更好的代码可维护性和更强大的异步渲染能力。同时为我们开发复杂的应用提供了更强大和灵活的工具和特性。

# 架构分层
此处分层的标准并非官方说法，只是为了深入理解 react, 在官方标准之外, 对其进行分解和剖析, 方便我们理解 react 架构.
1.  整个内核部分, 由 3 部分构成:

    1.  调度器  
        `scheduler`包, 核心职责只有 1 个, 就是执行回调.

        -   把`react-reconciler`提供的回调函数, 包装到一个任务对象中.
        -   在内部维护一个任务队列, 优先级高的排在最前面.
        -   循环消费任务队列, 直到队列清空.

    1.  构造器  
        `react-reconciler`包, 有 3 个核心职责:

        1.  装载渲染器, 渲染器必须实现[`HostConfig`协议](https://github.com/facebook/react/blob/v17.0.2/packages/react-reconciler/README.md#practical-examples)(如: `react-dom`), 保证在需要的时候, 能够正确调用渲染器的 api, 生成实际节点(如: `dom`节点).
        1.  接收`react-dom`包(初次`render`)和`react`包(后续更新`setState`)发起的更新请求.
        1.  将`fiber`树的构造过程包装在一个回调函数中, 并将此回调函数传入到`scheduler`包等待调度.

    1.  渲染器  
        `react-dom`包, 有 2 个核心职责:

        1.  引导`react`应用的启动(通过`ReactDOM.render`).
        1.  实现[`HostConfig`协议](https://github.com/facebook/react/blob/v17.0.2/packages/react-reconciler/README.md#practical-examples)([源码在 ReactDOMHostConfig.js 中](https://github.com/facebook/react/blob/v17.0.2/packages/react-dom/src/client/ReactDOMHostConfig.js)), 能够将`react-reconciler`包构造出来的`fiber`树表现出来, 生成 dom 节点(浏览器中), 生成字符串(ssr).

关系图一览：
现将内核 3 个包的主要职责和调用关系, 绘制到一张概览图上:
![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/eef5693f4e294e06823c71a857a979ab~tplv-k3u1fbpfcp-watermark.image?)

注意:

-   红色方块代表入口函数, 绿色方块代表出口函数.
-   package 之间的调用脉络就是通过板块间的入口和出口函数连接起来的.

通过此概览图, 基本可以表述 react 内核层的宏观结构. 后面的内容，基本基于此展开

# 工作循环
从上面的概览图中，可以看到有两个大的循环, 它们分别位于`scheduler`和`react-reconciler`包中:

暂且就将这两个循环分别表述为`任务调度循环`和`fiber构造循环`，下面对其进行一些简单的了解
1.  `任务调度循环`

源码位于[`Scheduler.js`](https://github.com/facebook/react/blob/v17.0.2/packages/scheduler/src/Scheduler.js), 它是`react`应用得以运行的保证, 它需要循环调用, 控制所有任务(`task`)的调度.

2.  `fiber构造循环`

源码位于[`ReactFiberWorkLoop.js`](https://github.com/facebook/react/blob/v17.0.2/packages/react-reconciler/src/ReactFiberWorkLoop.old.js), 控制 fiber 树的构造, 整个过程是一个深度优先遍历.

这两个循环对应的 js 源码不同于其他闭包(运行时就是闭包), 其中定义的全局变量, 不仅是该作用域的私有变量, 更用于`控制react应用的执行过程`.

# 区别与联系

1.  区别

    -   `任务调度循环`是以`二叉堆`（一种特殊的堆,`二叉堆` 是 `完全二叉树` 或者是 `近似完全二叉树`。）为数据结构, 循环执行`堆`的顶点, 直到`堆`被清空.
    -   `任务调度循环`的逻辑偏向宏观, 它调度的是每一个任务(`task`), 而不关心这个任务具体是干什么的(甚至可以将`Scheduler`包脱离`react`使用), 具体任务其实就是执行回调函数`performSyncWorkOnRoot`或`performConcurrentWorkOnRoot`.
    -   `fiber构造循环`是以`树`为数据结构, 从上至下执行深度优先遍历.
    -   `fiber构造循环`的逻辑偏向具体实现, 它只是任务(`task`)的一部分(如`performSyncWorkOnRoot`包括: `fiber`树的构造, `DOM`渲染, 调度检测), 只负责`fiber`树的构造.

1.  联系

    -   `fiber构造循环`是`任务调度循环`中的任务(`task`)的一部分. 它们是从属关系, 每个任务都会重新构造一个`fiber`树.

# 主干逻辑分析

其实两大循环的分工可以总结为: 大循环(任务调度循环)负责调度`task`, 小循环(fiber 构造循环)负责实现`task` .

react 运行的主干逻辑, 即将`输入转换为输出`的核心步骤, 实际上就是围绕这两大工作循环进行展开.

结合上文的宏观概览图(展示核心包之间的调用关系), 可以将 react 运行的主干逻辑进行概括:

1.  输入: 将每一次更新(如: 新增, 删除, 修改节点之后)视为一次`更新需求`(目的是要更新`DOM`节点).

1.  注册调度任务: `react-reconciler`收到`更新需求`之后, 并不会立即构造`fiber树`, 而是去调度中心`scheduler`注册一个新任务`task`, 即把`更新需求`转换成一个`task`.

1.  执行调度任务(输出): 调度中心`scheduler`通过`任务调度循环`来执行`task`(`task`的执行过程又回到了`react-reconciler`包中).

    -   `fiber构造循环`是`task`的实现环节之一, 循环完成之后会构造出最新的 fiber 树.
    -   `commitRoot`是`task`的实现环节之二, 把最新的 fiber 树最终渲染到页面上, `task`完成.

主干逻辑就是`输入到输出`这一条链路, 为了更好的性能(如`批量更新`, `可中断渲染`等功能), `react`在输入到输出的链路上做了很多优化策略, 比如本文讲述的`任务调度循环`和`fiber构造循环`相互配合就可以实现`可中断渲染`.

差不多循环和逻辑就是这样，通过这两个大循环概括出`react`运行的主干逻辑. `react-reconciler`和`Scheduler`包代码量多且逻辑复杂, 但实际上大部分都是服务于这个主干的。

# reconciler 运作流程
在开始讲解 `scheduler` 内容之前,我们先来了解一下 `reconciler` 的主要作用,它可以分为以下四个方面:
1.  输入: 暴露`api`函数(如: `scheduleUpdateOnFiber`), 供给其他包(如`react`包)调用.
1.  注册调度任务: 与调度中心(`scheduler`包)交互, 注册调度任务`task`, 等待任务回调.
1.  执行任务回调: 在内存中构造出`fiber树`, 同时与与渲染器(`react-dom`)交互, 在内存中创建出与`fiber`对应的`DOM`节点.
1.  输出: 与渲染器(`react-dom`)交互, 渲染`DOM`节点.

现在将这些功能(从输入到输出)串联起来，`reconciler` 的整个运作流程可以看做是一个固定的流程，流程用下图表示::

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b8b1f71bbcad46f4b2fbab90e5062a08~tplv-k3u1fbpfcp-watermark.image?)
图中的`1,2,3,4`步骤可以反映`react-reconciler`包`从输入到输出`的运作流程,这是一个固定流程, 每一次更新都会运行.

这边就大致说一下核心的步骤，如果想详细了解 `reconciler`的具体细节,以上功能的源码都在[ReactFiberWorkLoop.js](https://github.com/facebook/react/blob/v17.0.2/packages/react-reconciler/src/ReactFiberWorkLoop.old.js)中.

# React 中的优先级管理
基于`react@17.0.2`, `React`内部对于`优先级`的管理, 根据功能的不同分为`LanePriority`, `SchedulerPriority`, `ReactPriorityLevel`3 种类型，稍微展开说一下.
在React 17.0.2中，React内部对于优先级的管理主要分为三种类型：`LanePriority`（车道优先级）、`SchedulerPriority`（调度器优先级）和`ReactPriorityLevel`（React优先级等级）。

1.  `LanePriority`（车道优先级）：也可以称为fiber优先级，车道是React中用于区分更新类型的概念，其位于`react-reconciler`包。不同类型的更新任务会被分配到不同的车道中，这样可以在处理更新时根据车道的优先级进行区分，以确保高优先级的更新能够优先处理。车道优先级由`LanePriority`表示。
1.  `SchedulerPriority`（调度器优先级）：调度器是React内部用于安排更新任务执行的模块，其位于`scheduler`包。调度器优先级用于确定更新任务的调度顺序。具有更高优先级的任务将被优先调度执行，以提高响应性和用户体验。
1.  `ReactPriorityLevel`（React优先级等级）：位于`react-reconciler`包中的[`SchedulerWithReactIntegration.js`](https://github.com/facebook/react/blob/v17.0.2/packages/react-reconciler/src/SchedulerWithReactIntegration.old.js)，React还定义了一组优先级等级，用于在不同的React内部模块中进行任务调度和处理。这些等级按照优先级从高到低划分，并包括以下几个级别：`ImmediatePriority`（立即优先级）、`UserBlockingPriority`（用户阻塞优先级）、`NormalPriority`（普通优先级）、`LowPriority`（低优先级）、`IdlePriority`（空闲优先级）。

`reconciler`从输入到输出一共经历了4个阶段（接受输入，协调，渲染，提交）, 在每个阶段中都会涉及到与`优先级`相关的处理. 正是通过对`优先级`的灵活运用和管理, `React`实现了`可中断渲染`,`时间切片(time slicing)`,`异步渲染(suspense)`等特性，可以更好地控制和安排不同的更新任务，以提供更好的性能和用户体验。它能够确保高优先级的任务得到及时处理，而低优先级的任务则可以在适当的时机被调度执行，从而平衡应用的响应性和资源利用。

# React 调度原理(scheduler)
在整个 `React` 应用中,`shceduler` 是整个 `React` 运行时的大心脏,可以说理解了`scheduler`调度, 就基本把握了 React 的命门。

那么 `React` 是怎么把任务交给 `scheduler` 调度和控制执行呢?

先上`调度中心`最核心的代码： 在[SchedulerHostConfig.default.js](https://github.com/facebook/react/blob/v17.0.2/packages/scheduler/src/forks/SchedulerHostConfig.default.js)中.

该 js 文件一共导出了 8 个函数, 最核心的逻辑, 就集中在了这 8 个函数中 :

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/543625c6ed784031adfe4df0e189d1bf~tplv-k3u1fbpfcp-watermark.image?)

基于浏览器，简单分析一下：
1.  调度相关: 请求或取消调度

-   [requestHostCallback](https://github.com/facebook/react/blob/v17.0.2/packages/scheduler/src/forks/SchedulerHostConfig.default.js#L224-L230)
-   [cancelHostCallback](https://github.com/facebook/react/blob/v17.0.2/packages/scheduler/src/forks/SchedulerHostConfig.default.js#L232-L234)
-   [requestHostTimeout](https://github.com/facebook/react/blob/v17.0.2/packages/scheduler/src/forks/SchedulerHostConfig.default.js#L236-L240)
-   [cancelHostTimeout](https://github.com/facebook/react/blob/v17.0.2/packages/scheduler/src/forks/SchedulerHostConfig.default.js#L242-L245)

这 4 个函数源码很简洁, 非常好理解, 它们的目的就是请求执行(或取消)回调函数.重点介绍其中的`及时回调`：

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e2c4b5191b294a92a3e1035f779bd320~tplv-k3u1fbpfcp-watermark.image?)
![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6e08bbec696e42e39e49579609ffabcd~tplv-k3u1fbpfcp-watermark.image?)

这里可以看出来, 请求回调之后`scheduledHostCallback = callback`, 然后通过`MessageChannel`发消息的方式触发`performWorkUntilDeadline`函数, 最后执行回调`scheduledHostCallback`.

此处需要注意: `MessageChannel`在浏览器事件循环中属于`宏任务`, 所以调度中心永远是`异步执行`回调函数.

2.  时间切片(`time slicing`)相关: 执行时间分割, 让出主线程(把控制权归还浏览器, 浏览器可以处理用户输入, UI 绘制等紧急任务).

-   [getCurrentTime](https://github.com/facebook/react/blob/v17.0.2/packages/scheduler/src/forks/SchedulerHostConfig.default.js#L22-L24): 获取当前时间
-   [shouldYieldToHost](https://github.com/facebook/react/blob/v17.0.2/packages/scheduler/src/forks/SchedulerHostConfig.default.js#L129-L152): 是否让出主线程
-   [requestPaint](https://github.com/facebook/react/blob/v17.0.2/packages/scheduler/src/forks/SchedulerHostConfig.default.js#L154-L156): 请求绘制
-   [forceFrameRate](https://github.com/facebook/react/blob/v17.0.2/packages/scheduler/src/forks/SchedulerHostConfig.default.js#L168-L183): 强制设置 `yieldInterval`(从源码中的引用来看, 算一个保留函数)


![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f92c586bd00a493383ecf66621da676a~tplv-k3u1fbpfcp-watermark.image?)

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/15db3e6507304d20b1f4d4b82774dd13~tplv-k3u1fbpfcp-watermark.image?)

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ddc551f8bba7413f8bb6dd88831f6a64~tplv-k3u1fbpfcp-watermark.image?)
注意`shouldYieldToHost`的判定条件:

-   `currentTime >= deadline`: 只有时间超过`deadline`之后才会让出主线程(其中`deadline = currentTime + yieldInterval`).

    -   `yieldInterval`默认是`5ms`, 只能通过`forceFrameRate`函数来修改(事实上在 v17.0.2 源码中, 并没有使用到该函数).
    -   如果一个`task`运行时间超过`5ms`, 下一个`task`执行之前, 会把控制权归还浏览器.

-   `navigator.scheduling.isInputPending()`: 这 facebook 官方贡献给 Chromium 的 api, 用于判断是否有输入事件(包括: input 框输入事件, 点击事件等).

介绍完这 8 个内部函数, 最后浏览一下完整回调的实现performWorkUntilDeadline(逻辑很清晰, 在注释中解释):
```
const performWorkUntilDeadline = () => {
  if (scheduledHostCallback !== null) {
    const currentTime = getCurrentTime(); // 1. 获取当前时间
    deadline = currentTime + yieldInterval; // 2. 设置deadline
    const hasTimeRemaining = true;
    try {
      // 3. 执行回调, 返回是否有还有剩余任务
      const hasMoreWork = scheduledHostCallback(hasTimeRemaining, currentTime);
      if (!hasMoreWork) {
        // 没有剩余任务, 退出
        isMessageLoopRunning = false;
        scheduledHostCallback = null;
      } else {
        port.postMessage(null); // 有剩余任务, 发起新的调度
      }
    } catch (error) {
      port.postMessage(null); // 如有异常, 重新发起调度
      throw error;
    }
  } else {
    isMessageLoopRunning = false;
  }
  needsPaint = false; // 重置开关
};
```
给出调度中心的实现图示：

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9e20a173e061417cac1e0bedc9d3dbd6~tplv-k3u1fbpfcp-watermark.image?)

# 任务队列管理
上面详细说了请求和取消调度的实现原理. 调度的目的是为了消费任务, 接下来就具体分析任务队列是如何管理与实现的：
在[Scheduler.js](https://github.com/facebook/react/blob/v17.0.2/packages/scheduler/src/Scheduler.js)中, 维护了一个[taskQueue](https://github.com/facebook/react/blob/v17.0.2/packages/scheduler/src/Scheduler.js#L62), 任务队列管理就是围绕这个`taskQueue`展开.

```
// Tasks are stored on a min heap
var taskQueue = [];
var timerQueue = [];
```

注意:

-   `taskQueue`是一个小顶堆数组.
-   源码中除了`taskQueue`队列之外还有一个`timerQueue`队列. 这个队列是预留给延时任务使用的.
-   其中 `timerQueue` 表示要延迟执行的任务,`taskQueue` 表示现在就要执行的任务,这两个数组命名为 `Queue`,但是它实际上是一个 `二叉堆`。

## 创建任务
![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/feb194bd45be4203aad4198ecaf36893~tplv-k3u1fbpfcp-watermark.image?)

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/dcd9ae2cbd6a490c88cebc64c1162327~tplv-k3u1fbpfcp-watermark.image?)

## 消费任务
创建任务之后, 最后请求调度`requestHostCallback(flushWork)`, `flushWork`函数作为参数被传入调度中心内核等待回调. `requestHostCallback`函数在上文调度内核中已经介绍过了, 在调度中心中, 只需下一个事件循环就会执行回调, 最终执行`flushWork`.

在 `flushWork` 函数中,主要做的事情是对几个状态进行修改:

-   将 `isHostCallbackScheduled` 设置为 `false`,表示没有回调函数了;
-   如果 `isHostTimeoutScheduled` 为 `true`,将其设置为 `false`,并取消定时;
-   将 `isPerformingWork` 设置为 `true`,表示任务在执行中了;
-   调用 `workLoop(...)` 函数;

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/51d8634159944187a09c4769fdf507c3~tplv-k3u1fbpfcp-watermark.image?)

## workLoop
`flushWork`已经做了很多它自己能做的事情，他里面调用了`workLoop`函数去做了一些比如任务中断和任务恢复等事情.其实队列消费的主要逻辑就是在`workLoop`函数中, 就是前面所说的`任务调度循环`，我们来看看它的实现是怎么样的.

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/bcd512078acc414cbd65b2cf6ed59e31~tplv-k3u1fbpfcp-watermark.image?)

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cd9a6deaf85d40f79bf8e47b1c451795~tplv-k3u1fbpfcp-watermark.image?)
`workLoop`就是一个大循环, 虽然代码也不多, 但是非常精髓, 在此处实现了`时间切片(time slicing)`和`fiber树的可中断渲染`. 这 2 大特性的实现, 都集中于这个`while`循环.

这个函数的主要执行步骤有以下几个方面:
-   调用 `advanceTimers(...)` 函数把过期的任务从 `timerQueue(...)` 取出来转移到 `taskQueue` 里执行;
-   获取优先级最高的任务作为第一个处理的任务;
-   进入循环，在执行任务前，先看看还有没有时间;
-   如果没有时间跳出循环,返回 `true`,标明需要恢复任务;
-   如果有时间则正常执行任务,并保存任务的返回值;
-   如果返回值是函数,说明任务执行时长不够了,需要恢复;
-   如果返回值不是函数,说明已经执行完了,从队列中移除当前任务;
-   每次调度任务之后,都通过 `advanceTimers` 把过期的任务从 `timerQueue` 取出来转移 `taskQueue`，因为在执行过程中有可能部分任务也过期了;

在上面的代码中调用了 `shouldYieldToHost` 函数和`continuationCallback` 函数,前者的主要作用是判断当前时间片是否到期，后者的作用是任务的恢复和执行,不仅完成多个任务的时候会被打断,单个任务执行也会被打断,如果被打断了,那么就返回并等待下次执行。


每一次`while`循环的退出就是一个时间切片, 深入分析`while`循环的退出条件:

1.  队列被完全清空: 这种情况就是很正常的情况, 一气呵成, 没有遇到任何阻碍.

1.  执行超时: 在消费`taskQueue`时, 在执行`task.callback`之前, 都会检测是否超时, 所以超时检测是以`task`为单位.

    -   如果某个`task.callback`执行时间太长(如: `fiber树`很大, 或逻辑很重)也会造成超时
    -   所以在执行`task.callback`过程中, 也需要一种机制检测是否超时, 如果超时了就立刻暂停`task.callback`的执行.

## 时间切片原理

消费任务队列的过程中, 可以消费`1~n`个 task, 甚至清空整个 queue. 但是在每一次具体执行`task.callback`之前都要进行超时检测, 如果超时可以立即退出循环并等待下一次调用.

## 可中断渲染原理

在时间切片的基础之上, 如果单个`task.callback`执行时间就很长(假设 200ms). 就需要`task.callback`自己能够检测是否超时, 所以在 fiber 树构造过程中, 每构造完成一个单元, 都会检测一次超时([源码链接](https://github.com/facebook/react/blob/v17.0.2/packages/react-reconciler/src/ReactFiberWorkLoop.old.js#L1637-L1639)), 如遇超时就退出`fiber树构造循环`, 并返回一个新的回调函数(就是此处的`continuationCallback`)并等待下一次回调继续未完成的`fiber树构造`.

# 总结
也没啥好总结的感觉，调度原理差不多就是这些了，后面还剩下一个fiber树的构造和渲染。
1.  fiber树构造主要讲的是如何对处理好的fiber树进行构造循环处理（初次构建和对比更新）。有兴趣的同学可以看看`fiber树构造循环`(对应源码位于[ReactFiberWorkLoop 函数中](https://github.com/facebook/react/blob/v17.0.2/packages/react-reconciler/src/ReactFiberWorkLoop.old.js))
1.  fiber树渲染其实就是对构造好的fiber树进行进一步处理，作为最后的输出阶段, 是整个`reconciler 运作流程`的链路中最后一环。主要分为渲染前中后，里面使用到了`渲染优先级`和`调度优先级`，最终通过渲染器`react-dom`把最新的 DOM 对象渲染到界面上。
具体的`fiber树渲染`对应源码，整个逻辑都在[commitRoot 函数中](https://github.com/facebook/react/blob/v17.0.2/packages/react-reconciler/src/ReactFiberWorkLoop.old.js#L1879-L2254)

感觉啰嗦了一大堆，讲的主要是一些概念和理解上面的知识，对Fiber的一些知识问答请移步 [此处](https://juejin.cn/post/7260016275299975225)

`道阻且长，行则将至...`

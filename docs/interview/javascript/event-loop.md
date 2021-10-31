# 事件循环面试题

## 一、Promise面试题

```js
setTimeout(function () {
  console.log("setTimeout1");

  new Promise(function (resolve) {
    resolve();
  }).then(function () {
    new Promise(function (resolve) {
      resolve();
    }).then(function () {
      console.log("then4");
    });
    console.log("then2");
  });
});

new Promise(function (resolve) {
  console.log("promise1");
  resolve();
}).then(function () {
  console.log("then1");
});

setTimeout(function () {
  console.log("setTimeout2");
});

console.log(2);

queueMicrotask(() => {
  console.log("queueMicrotask1")
});

new Promise(function (resolve) {
  resolve();
}).then(function () {
  console.log("then3");
});
```

<details>
<summary>查看解析（点击展开）</summary>


```js
/**
promise1
2
then1
queueMicrotask1
then3
setTimeout1
then2
then4
setTimeout2
 */
```

我们还是通过画图的方式(更加直观)

![](https://gitee.com/itsandy/picgo-img/raw/master/JavaScript/Promise面试题流程一.png)

执行main script
1.  从上往下执行，遇到setTimeout（宏任务）这里称为s1放入宏任务中
2.  遇到new Promise（并不属于宏任务或者微任务）直接执行，打印promise1，调用resolve方法，会把后面then方法里面的回调函数放入微任务中（Promise的then回调）这里称为then1(只是放入微任务中，并不是直接执行)
3.  setTimeout（宏任务）这里称为s2放入宏任务中
4.  打印2
5.  queueMicrotask（微任务）放入微任务中（q1）
6.  new Promise立即执行，调用resolve方法，将then里的回调函数放入微任务中（then3）

main script就执行完了，接下来执行微任务

1.  执行then1：打印then1
2.  执行q1：打印queueMicrotask1
3.  执行then3,打印then3

微任务就执行完了，接下来执行宏任务

1. 执行s1：打印setTimeout1，new promise直接执行，调用resolve方法，将then里的回调放入微任务中（then4），打印then2
2. 执行了一次宏任务后，会去检查微任务中是否有任务需要执行，微任务中有then4，执行then4，打印then4
3. 执行s2：打印setTimeout2

</details>

## 二、async/await面试题

在做async/await的面试题前，我们得先看看这道题

```js
async function foo() {
  console.log(222);
  return new Promise(resolve => {
    resolve()
  })
}

async function bar() {
  console.log(111);
  await foo()
  console.log(333);
}

bar()
console.log(444);
```
<details>
<summary>查看解析（点击展开）</summary>


```js
/**
111
222
444
333
 */
```
1.  执行bar函数（默认执行一个异步函数是跟普通的函数是一样的）打印111
2.  执行foo函数（你不要觉得它在await后面就不会执行），打印222，返回一个promise
3.  打印444(为什么不是打印333，在async的文章中说过，这333其实是在foo调用了then以后才会执行
4.  打印333

</details>

不是很理解的，先去看看[async](javascript/es-next/es8?id=_63-await关键字)文章


```js
async function async1 () {
  console.log('async1 start')
  await async2();
  console.log('async1 end')
}
 
async function async2 () {
  console.log('async2')
}

console.log('script start')
 
setTimeout(function () {
  console.log('setTimeout')
}, 0)
 
async1();
 
new Promise (function (resolve) {
  console.log('promise1')
  resolve();
}).then (function () {
  console.log('promise2')
})

console.log('script end')
```

<details>
<summary>查看解析（点击展开）</summary>


```js
/**
script start
async1 start
async2
promsie1
script end
async1 end
promise2
setTimeout
 */
```

![](https://gitee.com/itsandy/picgo-img/raw/master/JavaScript/async面试题流程一.png)

执行main script
1.  打印script start
2.  setTimeout放入宏任务中
3.  调用async1函数，打印async1 start，调用async2函数，打印async2，返回一个promise，resolve的值是undefined，把async1 end放入微任务中
4.  new promise直接执行，打印promise1，调用resolve方法，将then里的回调放入微任务中
5.  打印script end

main script就执行完了，接下来执行微任务
1.  打印async1 end
2.  打印promise2

微任务就执行完了，接下来执行宏任务
1.  打印setTimeout


</details>

## 三、promise超难面试题

这道题其实能做出来的人很少很少

我们来做一下就可以知道这道题的难度了

```js
Promise.resolve().then(() => {
  console.log(0);
  return 4
}).then((res) => {
  console.log(res)
})


Promise.resolve().then(() => {
  console.log(1);
}).then(() => {
  console.log(2);
}).then(() => {
  console.log(3);
}).then(() => {
  console.log(5);
}).then(() =>{
  console.log(6);
})
```

<details>
<summary>查看解析（点击展开）</summary>


```js
/**
0
1
4
2
3
5
6
 */
```

1.  将0，return 4放入微任务中
2.  将1放入微任务中
3.  打印0，return 4（相对于resolve（4））（then里面return值会被作为新的promise的resolve值在下一次链式调用的then里面拿到结果的），将then 打印res放入微任务
4.  打印1，return undefined（then里面return值会被作为新的promise的resolve值在下一次链式调用的then里面拿到结果的），将then里面的2放入微任务
5.  打印4，因为微任务前面还有个4队列，然后return undefined（then里面return值会被作为新的promise的resolve值在下一次链式调用的then里面拿到结果的），将then里的3放入队列

依次类推。。。

</details>

可能并没有觉得很难，那我们来看下面这道题

```js
Promise.resolve().then(() => {
  console.log(0);
  return {
    then(resolve){
      resolve(4)
    }
  }
}).then((res) => {
  console.log(res)
})


Promise.resolve().then(() => {
  console.log(1);
}).then(() => {
  console.log(2);
}).then(() => {
  console.log(3);
}).then(() => {
  console.log(5);
}).then(() => {
  console.log(6);
})
```
这次不返回4了。返回了一个thenable

<details>
<summary>查看解析（点击展开）</summary>


```js
/**
0
1
2
4
3
5
6
 */
```

按理来说，一个是和return 4差不多的，但是这里打印的结果不同

如果你这里return是一个thenable，它会多加一次微任务（相对于它会往后推一次）

但是它为什么会往后推喃？你会发现thenable是一个函数，这里我们并没有在函数里面做什么操作，如果在函数里面我们做了大量的耗时操作的话，它就会一直堵塞在这里，后面的代码就不会执行了，所以它就往后推迟了一下（当然这是我的猜测）

</details>

如果我们return的是promise.resolve的话，它会往后推两次

```js
Promise.resolve().then(() => {
  console.log(0);
  return Promise.resolve(4)
})


Promise.resolve().then(() => {
  console.log(1);
}).then(() => {
  console.log(2);
}).then(() => {
  console.log(3);
}).then(() => {
  console.log(5);
}).then(() => {
  console.log(6);
})

/**
0
1
2
3
4
5
6
 */
```

## 四、Node执行面试题

```js
async function async1() {
  console.log('async1 start')
  await async2()
  console.log('async1 end')
}

async function async2() {
  console.log('async2')
}

console.log('script start')

setTimeout(function () {
  console.log('setTimeout0')
}, 0)

setTimeout(function () {
  console.log('setTimeout2')
}, 300)

setImmediate(() => console.log('setImmediate'));

process.nextTick(() => console.log('nextTick1'));

async1();

process.nextTick(() => console.log('nextTick2'));

new Promise(function (resolve) {
  console.log('promise1')
  resolve();
  console.log('promise2')
}).then(function () {
  console.log('promise3')
})

console.log('script end')
```

<details>
<summary>查看解析（点击展开）</summary>


```js
/**
script start
async1 start
async2
promise1
promise2
script end
nextTick1
nextTick2
async1 end
promise3
setTimeout0
setImmediate
setTimeout2
 */
```

![](https://gitee.com/itsandy/picgo-img/raw/master/JavaScript/node执行面试题.png)

执行main script
1.  打印script start
2.  setTimeout0放入宏任务中放入宏任务里的timer queue队列中
3.  setTimeout2在300ms后放入宏任务里的timer queue队列中
4.  setImmediate放入宏任务里的check queue队列中
5.  nextTick1放入微任务里的next tick queue队列中
6.  调用async1函数，打印async1 start，调用async2，打印async2，将async1 end放入微任务里的other queue队列中
7.  nextTick2放入微任务里的next tick queue队列中
8.  new promise直接执行，打印promise1，调用resolve方法，将promise3放入微任务里的other queue队列中，打印promise2
9.  打印script end

main script就执行完了，接下来执行微任务
先执行next tick queue队列
1.  打印nextTick1
2.  打印nextTick2
再执行next tick queue队列
3.  打印async1 end
4.  打印promise3

微任务就执行完了，接下来执行宏任务
先执行timer queue队列
1.  打印setTimeout0
再执行check queue队列
2.  打印setImmediate

最后打印setTimeout2（因为它是300ms后才会被加入到next tick queue队列）


</details>
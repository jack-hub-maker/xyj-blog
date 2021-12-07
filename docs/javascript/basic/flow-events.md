# 浏览器事件解析
## 一、事件监听

我们讲到了JavaScript脚本和浏览器之间交互时，浏览器给我们提供的BOM、DOM等一些对象模型。
- 事实上还有一种需要和浏览器经常交互的事情就是事件监听：
- 浏览器在某个时刻可能会发生一些事件，比如鼠标点击、移动、滚动、获取、失去焦点、输入内容等等一系列
的事件；

我们需要以某种方式（代码）来对其进行响应，进行一些事件的处理；

在Web当中，事件在浏览器窗口中被触发，并且通过绑定到某些元素上或者浏览器窗口本身，那么我们就可以
给这些元素或者window窗口来绑定事件的处理程序，来对事件进行监听。

如何进行事件监听呢？

### 1.在script中直接监听

```html
<button onclick="btnClick">按钮</button>
<script>
  function btnClick() {
    console.log('按钮发生了点击')
  }
</script>
```
### 2.通过元素的on来监听事件

```html
<button class="btn">按钮</button>
<script>
  const btn = document.querySelector('.btn')
  btn.onclick = () => {
    console.log('按钮发生了点击')
  }
</script>
```

### 3.通过EventTarget中的addEventListener来监听

```html
<button class="btn">按钮</button>
<script>
  const btn = document.querySelector('.btn')
  // 可以多次监听
  btn.addEventListener('click', () => {
    console.log('按钮发生了点击1')
  })
  btn.addEventListener('click', () => {
    console.log('按钮发生了点击2')
  })
  btn.addEventListener('click', () => {
    console.log('按钮发生了点击3')
  })
</script>
```

## 二、事件流

事实上对于事件有一个概念叫做事件流，为什么会产生事件流呢？

我们可以想到一个问题：当我们在浏览器上对着一个元素点击时，你点击的不仅仅是这个元素本身；

这是因为我们的HTML元素是存在父子元素叠加层级的；

比如一个span元素是放在div元素上的，div元素是放在body元素上的，body元素是放在html元素上的；

```html
<div>
  <span>span元素</span>
</div>
<script>
  const span = document.querySelector('span')
  const div = document.querySelector('div')

  span.addEventListener('click', () => {
    console.log('span元素发生了点击')
  })
  div.addEventListener('click', () => {
    console.log('div元素发生了点击')
  })
  document.body.addEventListener('click', () => {
    console.log('body发生了点击')
  })
</script>
```
## 三、事件冒泡和事件捕获

我们会发现默认情况下事件是从最内层的span向外依次传递的顺序，这个顺序我们称之为**事件冒泡**（Event
Bubble）。

事实上，还有另外一种监听事件流的方式就是从外层到内层（body -> span），这种称之为**事件捕获**（Event
Capture）；

可以通过[addEventListener](https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget/addEventListener)的第三个参数来指定监听元素是否进行事件捕获

```js
span.addEventListener('click', () => {
  console.log('事件捕获:span元素发生了点击')
}, true)
div.addEventListener('click', () => {
  console.log('事件捕获:div元素发生了点击')
}, true)
document.body.addEventListener('click', () => {
  console.log('事件捕获:body发生了点击')
}, true)
```

为什么会产生两种不同的处理流呢？
- 这是因为早期浏览器开发时，不管是IE还是Netscape公司都发现了这个问题，但是他们采用了完全相反的事
件流来对事件进行了传递；
- IE采用了事件冒泡的方式，Netscape采用了事件捕获的方式；

## 四、冒泡和捕获的顺序

如果我们同时有事件冒泡和时间捕获的监听，那么会优先监听到事件捕获的(`先捕获后监听`)

![](https://gitee.com/itsandy/picgo-img/raw/master/JavaScript/冒泡和捕获的顺序.png)


## 五、事件对象event

当一个事件发生时，就会有和这个事件相关的很多信息：
- 比如事件的类型是什么，你点击的是哪一个元素，点击的位置是哪里等等相关的信息；
- 那么这些信息会被封装到一个Event对象中；
- 该对象给我们提供了想要的一些属性，以及可以通过该对象进行某些操作；

常见的属性：
- type：事件的类型；
- target：当前事件发生的元素；
- currentTarget：当前处理事件的元素；
- offsetX、offsetY：点击元素的位置；

```js
span.addEventListener('click', (event) => {
  console.log('事件冒泡:span元素发生了点击', event.type)
})
div.addEventListener('click', (event) => {
  console.log('事件冒泡:div元素发生了点击', event.target)
})
document.body.addEventListener('click', (event) => {
  console.log('事件冒泡:body发生了点击', event.currentTarget, event.offsetX, event.offsetY)
})
```

常见的方法：
- preventDefault：取消事件的默认行为；
- stopPropagation：阻止事件的进一步传递；

```js
a.addEventListener('click', (event) => {
  event.preventDefault()
})
span.addEventListener('click', () => {
  console.log('事件冒泡:span元素发生了点击')
  event.stopPropagation()
})
div.addEventListener('click', () => {
  console.log('事件冒泡:div元素发生了点击')
})
document.body.addEventListener('click', () => {
  console.log('事件冒泡:body发生了点击')
})
```

还有很多的[事件参考](https://developer.mozilla.org/zh-CN/docs/Web/Events)（这里就不一一阐述了）
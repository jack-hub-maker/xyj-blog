# DOM操作架构

浏览器是用来展示网页的，而网页中最重要的就是里面各种的标签元素，JavaScript很多时候是需要操作这些元素的。
- JavaScript如何操作元素呢？通过Document Object Model（[DOM](https://developer.mozilla.org/zh-CN/docs/Web/API/Document_Object_Model)文档对象模型）。
- DOM给我们提供了一系列的模型和对象，让我们可以方便的来操作Web页面。


![](https://gitee.com/itsandy/picgo-img/raw/master/JavaScript/DOM的架构.png)

## 一、EventTarget

因为都继承自[EventTarget](https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget)，所以也可以使用EventTarget的方法：

```js
document.addEventListener('click', () => {
  console.log('document发生了点击')
})

const btn = document.querySelector('button')
btn.addEventListener('click', () => {
  console.log('按钮发生了点击')
})
```

## 二、Node节点

所有的DOM节点类型都继承自[Node](https://developer.mozilla.org/zh-CN/docs/Web/API/Node)接口。

Node有几个非常重要的属性：
- nodeName：node节点的名称。
- nodeType：可以区分节点的类型。
- nodeValue：node节点的值；
- childNodes：所有的子节点；

```js
console.log(document.nodeName)
console.log(document.nodeType)
console.log(document.nodeValue)
console.log(document.childNodes)

const div = document.querySelector('div')
console.log(div.nodeName)
console.log(div.nodeType)
console.log(div.nodeValue)
console.log(div.childNodes)
```

## 三、Document

[Document](https://developer.mozilla.org/zh-CN/docs/Web/API/Document)节点表示的整个载入的网页，我们来看一下常见的属性和方法：

```js
// 属性
console.log(document.title)
console.log(document.body)
console.log(document.children)
console.log(document.location)

// 方法
// 1.创建和添加元素
const button = document.createElement('button')
document.body.appendChild(button)
// 2.删除元素
setTimeout(() => {
  document.body.removeChild(button)
}, 2000);
// 3.获取元素
const titleEl1 = document.getElementById()
const titleEl2 = document.getElementsByName()
const titleEl3 = document.getElementsByTagName()
const titleEl4 = document.querySelector()
const titleEl5 = document.querySelectorAll()
```

## 四、Element

我们平时创建的div、p、span等元素在DOM中表示为Element元素，我们来看一下常见的属性和方法：

```html
<h2>Hello World</h2>
<script>
  const title = document.querySelector('h2')

  // 获取子元素
  console.log(title.children)

  // tagName
  console.log(title.tagName)

  // id/class
  console.log(title.id)
  console.log(title.className)
  console.log(title.classList)

  // 宽度和高度
  console.log(title.clientHeight)
  console.log(title.clientWidth)

  // 距离边框的距离
  console.log(title.offsetLeft)
  console.log(title.offsetTop)

  // 操作属性
  title.setAttribute('name', 'tao')
  console.log(title.getAttribute('name'))
</script>
```

!> 其实DOM的东西是非常非常多的，我们不可能记住全部的AP，掌握一些常用的属性和方法就好了，忘了查文档就行

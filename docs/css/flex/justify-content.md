# justify-content

### 含义

决定 flex items 在 main start 上的对齐方式

### 取值

- flex-start：与main start对齐

- flex-end：与main end对齐

- center：居中对齐

- space-between：flex items之间的距离相等，main start、main end两端对齐

- space-around：flex items之间的距离相等，与main start、main end之间的距离等于flex items之间的距离

- space-evenly：flex items之间的距离相等，main start、main end之间的距离是flex items之间距离的一半

### 例子

HTML

```html
<div class="box">
  <div class="item item1">item1</div>
  <div class="item item2">item2</div>
  <div class="item item3">item3</div>
</div>
```

CSS

```css
.box {
  width: 500px;
  height: 500px;
  background-color: orange;
  display: flex;
  margin: 0 auto;
  justify-content: flex-start;
}

.item {
  width: 100px;
  height: 100px;
  text-align: center;
  line-height: 100px;
  color: #fff;
}

.item1 {
  background-color: red;
}

.item2 {
  background-color: blue;
}

.item3 {
  background-color: green;
}
```

### 结果

flex-start

![1.png](https://img10.360buyimg.com/ddimg/jfs/t1/185559/4/20373/4069/6120c0ceEfbad6d8b/3504ed4e4ea14cdf.png)

flex-end

![2.png](https://img14.360buyimg.com/ddimg/jfs/t1/181002/27/20383/4073/6120c0cdE4905d63c/ca27fe018d0c4c51.png)

center

![3.png](https://img11.360buyimg.com/ddimg/jfs/t1/194460/36/19242/4069/6120c0cdE99c16c0c/e94875d2eb311858.png)

space-between

![4.png](https://img10.360buyimg.com/ddimg/jfs/t1/181343/21/20447/4042/6120c0cdEa6f17c0b/3948fe70fcc48d5a.png)

space-around

![5.png](https://img11.360buyimg.com/ddimg/jfs/t1/194928/17/19408/4144/6120c10dE6d16ec5e/5fd0dbee4612166c.png)

space-evenly

![6.png](https://img12.360buyimg.com/ddimg/jfs/t1/187320/21/19479/4140/6120c10dE52c5e03f/0f4733c9e864e402.png)

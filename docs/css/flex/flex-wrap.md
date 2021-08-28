# flex-wrap

### 含义

决定 flex items 是单行还是多行

### 取值

- nowrap：单行（默认）

- wrap：多行

### 例子

HTML

```html
<div class="box">
  <div class="item item1">item1</div>
  <div class="item item2">item2</div>
  <div class="item item3">item3</div>
  <div class="item item3">item3</div>
  <div class="item item3">item3</div>
  <div class="item item3">item3</div>
  <div class="item item3">item3</div>
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
  flex-wrap: wrap;
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

nowrap

![1.png](https://img12.360buyimg.com/ddimg/jfs/t1/179319/9/20371/5337/61211817Eaec92d13/feb3a70b9b853f5a.png)

wrap

![2.png](https://img11.360buyimg.com/ddimg/jfs/t1/195881/21/19521/4892/61211817Eb413cf43/43cf459b77debb1b.png)

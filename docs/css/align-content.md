# align-content

### 含义

决定多行 flex items 在 cross axis 上的对齐方式，用法与 justify-content 类似

### 取值

- flex-start：与 corss start 对齐

- flex-end：与 corss end 对齐

- center：居中对齐

- space-between：flex items 之间的距离相等，与 cross start、cross end 两端对齐

- space-around：flex items 之间的距离相等，flex items 与 cross start、corss end 之间的距离是 flex items 之间距离的一半

- space-evenly：flex items 之间的距离相等，flex items 与 cross start、corss end 之间的距离等于 flex items 之间的距离

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
  align-content: flex-start;
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

flex-start

![1.png](https://img13.360buyimg.com/ddimg/jfs/t1/179102/38/20531/4914/61211bdaEe6577ac0/9f7b761f85ad108f.png)

flex-end

![2.png](https://img14.360buyimg.com/ddimg/jfs/t1/197710/30/4351/4894/61211bdaEcac6b69e/fe3385e00df530b5.png)

center

![3.png](https://img10.360buyimg.com/ddimg/jfs/t1/199686/33/4146/4907/61211bdaEd842ab3a/903671a60ee6a2fb.png)

space-between

![4.png](https://img11.360buyimg.com/ddimg/jfs/t1/187575/17/19619/4925/61211bdaEe32432b0/081619490c598d56.png)

space-around

![5.png](https://img10.360buyimg.com/ddimg/jfs/t1/178265/7/20348/4918/61211be7Ed40f2cc5/6858a9d37dfcd0ff.png)

space-evenly

![6.png](https://img14.360buyimg.com/ddimg/jfs/t1/188127/4/19467/4983/61211be7Ecc0652fe/3391dc0e2b52060c.png)

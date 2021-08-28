# align-items

### 含义

决定 flex items 在 cross axis 上的对齐方式

### 取值

- flex-start：与 corss start 对齐

- flex-end：与 cross end 对齐

- center：居中对齐

- baseline：基线对齐（flex 中的基线是以第一行文字为基线）

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
  align-items: flex-start;
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

![1.png](https://img11.360buyimg.com/ddimg/jfs/t1/182036/21/20356/4087/6120d30cE4b2481c6/e01c971647ae88ef.png)

flex-end

![2.png](https://img11.360buyimg.com/ddimg/jfs/t1/205886/6/2341/4178/6120d30bEfbb46a05/93aad2651d71db8f.png)

center

![3.png](https://img14.360buyimg.com/ddimg/jfs/t1/178191/31/20263/4091/6120d30bEe014c13e/5bb4af2fefe18388.png)

baseline

![4.png](https://img12.360buyimg.com/ddimg/jfs/t1/191243/24/19297/4156/6120d30bE0651e882/477ec06b3fabf0b9.png)

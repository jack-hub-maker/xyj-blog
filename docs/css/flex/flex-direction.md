# flex-direction

### 含义

flex-direction 决定**主轴**的方向

### 取值

- row

- row-reverse

- column

- column-reverse

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
  flex-direction: row;
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

row

![1.png](https://img12.360buyimg.com/ddimg/jfs/t1/188496/29/19469/4144/6120bb27E0b7991b7/801de3945a68937a.png)

row-reverse

![2.png](https://img13.360buyimg.com/ddimg/jfs/t1/177715/40/20563/4091/6120bb27E9d328f73/45ab8faaa9964b72.png)

column

![3.png](https://img14.360buyimg.com/ddimg/jfs/t1/206140/29/2375/4158/6120bb27E171930de/3917728ad99547a7.png)

column-reverse

![4.png](https://img11.360buyimg.com/ddimg/jfs/t1/182441/16/20520/4253/6120bb27E6ba076aa/cafd7ebf246683f0.png)
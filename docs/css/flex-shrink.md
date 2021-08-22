# flex-shrink

### 含义

决定 flex items 如何收缩

### 取值

默认值为 1，可以设置任意非负数字（正小数，正整数，0）

当 flex container 在 main axis 方向上超过 flex container 的 size，flex-shrink 属性才会生效

如果 flex items 的 flex-shrink 总和 sum>1，每个 flex items 收缩的 size 为 flex items 超出 flex container 的 size\*收缩比例/所有 flex items 的收缩比例之和

如果 flex items 的 flex-shrink 总和 sun<=1，每个 flex items 收缩的 size 为 flex items 超过 flex container 的 size*sum * 收缩比例/所有 flex items 的收缩比例之和

收缩比例 = flex-shrink\*flex items 的 base size

base size 就是 flex items 放入 flex container 之前的 size

flex items 收缩后的最终 size 不得小于 min-width\min-height

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
}

.item {
  width: 200px;
  height: 100px;
  text-align: center;
  line-height: 100px;
  color: #fff;
}

.item1 {
  background-color: red;
  flex-shrink: 1;
}

.item2 {
  background-color: blue;
  flex-shrink: 2;
}

.item3 {
  background-color: green;
  flex-shrink: 2;
}
```

### 结果

![image.png](https://img12.360buyimg.com/ddimg/jfs/t1/201713/24/2521/13224/6121fb17E49c72c9f/4c12a157f392e5bf.png)

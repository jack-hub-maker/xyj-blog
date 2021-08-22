# flex-grow

### 含义

决定 flex items 如何扩展

### 取值

默认值为 0，可以设置任意非负数字（正小数，正整数，0）

当 flex container 在 main axis 方向上有剩余 size 时，flex-grow 属性才会生效

如果 flex items 的 flex-grow 总和 sum>1，每个 flex items 扩展的 size 为 flex container 的剩余 size\*flex-grow/sum

如果 flex items 的 flex-grow 总和 sun<=1，每个 flex items 扩展的 size 为 flex container 的剩余 size \* flex-grow

flex items 扩展后的最终 size 不得超过 max-width\max-height

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
  width: 100px;
  height: 100px;
  text-align: center;
  line-height: 100px;
  color: #fff;
}

.item1 {
  background-color: red;
  flex-grow: 2;
}

.item2 {
  background-color: blue;
  flex-grow: 3;
}

.item3 {
  background-color: green;
  flex-grow: 5;
}
```

### 结果

![image.png](https://img11.360buyimg.com/ddimg/jfs/t1/195429/39/19507/11994/6121f771E5e331934/9b21abc490b731c4.png)

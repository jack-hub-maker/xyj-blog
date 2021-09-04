# 定位

## 标准流

默认情况下，元素都是按照 normal flow（标准流，常规流，正常流，文档流）进行排布

按照从左往右，从上往下按顺序依次排列

默认情况下，互相之间不存在层叠现象

## margin、padding 定位

在标准流中，可以使用 margin、padding 对元素进行定位

其中 margin 还可以设置负数

但是这样有个比较明显的缺点

- 设置有个元素的 margin 或者 padding，通常会影响到标准流中其它元素的定位效果
- 不便于实现元素层叠的效果

## CSS 属性-position

利用 position 属性可以对元素进行定位，常用取值有 4 个

- static：静态定位（默认）
- [relative](css/position/relative)：相对定位
- [absolute](css/position/absolute)：绝对定位
- [fixed](css/position/fixed)：固定定位

## 绝对定位技巧

> 请看完前面 3 个定位属性再回来看这个技巧

绝对定位元素：position 值为 absolute 或者 fixed 的元素

对于绝对定位元素来说

- 定位元素对象的宽度 = left+right+margin-left+margin-right+绝对定位元素的实际占用宽度
- 定位参照对象的高度 = top+bottom+margin-top+margin-bottom+绝对定位元素的实际占用高度

这个有什么用喃？

如果希望绝对定位元素的宽高和定位参照对象一样，可以给绝对定位元素设置以下属性

- left：0，right：0，top：0，bottom：0，margin：0

如果希望绝对定位元素在定位参照对象中居中显示，可以给绝对定位元素设置以下属性

- left：0，right：0，top：0，bottom：0，margin：auto
- 另外，还需要设置具体的宽高值（宽高小于定位参照对象的宽高）

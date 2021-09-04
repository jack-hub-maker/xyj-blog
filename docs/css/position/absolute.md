# absolute-绝对定位

元素脱离标准流

可以通过 left、right、top、bottom 进行定位

定位参照对象是**最邻近**的定位祖先元素

如果找不到这样的祖先元素，参照对象是**视口**

定位元素：position 值不为 static 的元素，也就是 position 的值为 relative、absolute、fixed 的元素

## 脱标元素

- 可以随意设置宽高
- 宽高默认由内容决定
- 不再受标准流的约束
- 不再给父元素汇报宽高数据

也就是说脱标之后的元素在某些情况下会修改 display 值的计算值

一般来说都会变为 block，但是也有某些特殊情况，详情可见：https://developer.mozilla.org/zh-CN/docs/Web/CSS/float

但是你可能有些疑惑，display 为 block 了，那为什么宽度没有 100%喃

这是因为 display 为 block 的时候，width 和 height 为 auto，auto 是一个特别的属性，相对于父元素自适应

脱标后的元素它就不知道它的父元素是谁了，就会出现一个现象：包裹内容（内容是多宽就是多宽，多高就多高）

## 子绝父相

在绝大数情况下，子元素的绝对定位都是相对于父元素进行定位

如果希望子元素相对于父元素进行定位，又不希望父元素脱标，常用解决方案是：

- 父元素设置 position：relative（让父元素成为定位元素，而且父元素不脱标）
- 子元素设置 position：absolute
- 简称为子绝父相

应用场景：可以利用 absolute 做各种各样的效果

![image.png](https://img11.360buyimg.com/ddimg/jfs/t1/198642/21/6594/268273/61331561E931659a9/c1ea1deb31149316.png)

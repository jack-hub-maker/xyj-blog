# Grid 布局

CSS 网格是一个用于 web 的二维布局系统。利用网格，你可以把内容按照行与列的格式进行排版。另外，网格还能非常轻松地实现一些复杂的布局

## 一、grid 容器

![](https://gitee.com/itsandy/picgo-img/raw/master/css/grid容器.png)

grid 布局是将容器划分成了`行和列`，产生了一个个的网格，我们可以将网格元素放在与这些行和列相关的位置上，从而达到我们布局的目的。

![](https://gitee.com/itsandy/picgo-img/raw/master/css/grid架构.png)

### 1.1 grid-template-rows

[grid-template-rows](https://developer.mozilla.org/zh-CN/docs/Web/CSS/grid-template-rows) 设置行高

### 1.2 grid-template-columns

[grid-template-columns](https://developer.mozilla.org/zh-CN/docs/Web/CSS/grid-template-columns) 设置列宽

我们来看这个案例

```html
<div class="box">
  <div>1</div>
  <div>2</div>
  <div>3</div>
</div>
```

```css
.box {
  width: 300px;
  height: 300px;
  background-color: blue;
  display: grid;
  grid-template-rows: 100px 100px 100px;
  grid-template-columns: 100px 100px;
}
.box div:nth-child(1) {
  background-color: orange;
}

.box div:nth-child(2) {
  background-color: pink;
}

.box div:nth-child(3) {
  background-color: red;
}
```

整个盒子就是一个三行两列的布局了

![](https://gitee.com/itsandy/picgo-img/raw/master/css/grid-template-columns.png)

定义网格还提供了一个专门的单位：`fr`

fr 其实就是一个比例值，我们来看这个例子

```css
grid-template-rows: 1fr 1fr 1fr;
grid-template-columns: 1fr 1fr;
```

我们会发现这是容器划分为了三行两列

fr 单位代表网格容器中可用空间的一等份。

比如上面的例子就是行 300px，一共存在 3fr,每一份就去 1/3 就是 100px

### 1.3 grid-template-areas

[grid-template-areas](https://developer.mozilla.org/zh-CN/docs/Web/CSS/grid-template-areas) 合并网格以及网格命名

```css
grid-template-areas:
  "a b c"
  "a c b"
  "b a c";
```

这只是给网格不同的区域添加上了名字，要想真正使用还得 item 使用对应的属性

移步[grid-area](css/grid?id=_27-grid-area)

### 1.4 grid-template

[grid-template](https://developer.mozilla.org/zh-CN/docs/Web/CSS/grid-template)是 grid-template-rows,grid-template-columns,grid-template-areas 三者结合的简写

```css
grid-template:
  "a b b" 1fr
  "a b b" 1fr
  "c c c" 1fr
  / 1fr 1fr 1fr;
```

第一次看起来感觉好怪啊,因人而异吧

### 1.5 row-gap

[row-gap](https://developer.mozilla.org/zh-CN/docs/Web/CSS/row-gap) 行间距

### 1.6 column-gap

[column-gap](https://developer.mozilla.org/zh-CN/docs/Web/CSS/column-gap) 列间距

?>如果你以前学过 grid 的话可能会使用 grid-row-gap 和 grid-column-gap 属性,目前官方**更推荐**使用 row-gap 和 column-gap,官方其实也有明确的说到

![](https://gitee.com/itsandy/picgo-img/raw/master/css/为什么使用column-gap.png)

```css
grid-template-rows: 1fr 1fr 1fr;
grid-template-columns: 1fr 1fr;
row-gap: 20px;
column-gap: 20px;
```

![](https://gitee.com/itsandy/picgo-img/raw/master/css/column-gap.png)

### 1.7 gap

[gap](https://developer.mozilla.org/zh-CN/docs/Web/CSS/gap)是行间距和列间距的简写

```css
gap: 20px 20px;
```

![](https://gitee.com/itsandy/picgo-img/raw/master/css/column-gap.png)

### 1.8 justify-content

[justify-content](https://developer.mozilla.org/zh-CN/docs/Web/CSS/justify-content) 整体内容水平对齐

### 1.9 align-content

[align-content](https://developer.mozilla.org/zh-CN/docs/Web/CSS/align-content) 整体内容垂直对齐

### 1.10 place-content

[place-content](https://developer.mozilla.org/zh-CN/docs/Web/CSS/align-content) 是整体内容水平对齐和垂直对齐的简写

```css
place-content: center;
```

![](https://gitee.com/itsandy/picgo-img/raw/master/css/place-content.png)

### 1.11 justufy-items

[justufy-items](https://developer.mozilla.org/zh-CN/docs/Web/CSS/justify-items) items 水平对齐

### 1.12 align-items

[align-items](https://developer.mozilla.org/zh-CN/docs/Web/CSS/align-items) items 垂直对齐

### 1.13 place-items

[place-items](https://developer.mozilla.org/zh-CN/docs/Web/CSS/place-items)是水平对齐和垂直对齐的简写

```css
place-items: center center;
```

![](https://gitee.com/itsandy/picgo-img/raw/master/css/place-items.png)

### 1.14 grid-auto-flow

之前我们显示都是提前设置好了行和列的,如果在原来的基础上多增加了几个,会怎么样喃

![](https://gitee.com/itsandy/picgo-img/raw/master/css/隐式网格.png)

我们把 123 称之为显式网格,把 45 称之为`隐式网格`,这个隐式网格的排序是按照什么来规定的喃?也就要提到下面讲的这些属性

[grid-auto-flow](https://developer.mozilla.org/zh-CN/docs/Web/CSS/grid-auto-flow) 控制隐式网格的排序

默认 grid-auto-flow:row 也就是行产生隐式网格

```css
grid-auto-flow: column;
```

![](https://gitee.com/itsandy/picgo-img/raw/master/css/grid-auto-flow.png)

### 1.15 gird-auto-rows

你会发现默认隐式网格是会拉伸的,我们可以通过 [gird-auto-rows](https://developer.mozilla.org/zh-CN/docs/Web/CSS/grid-auto-rows) 来控制隐式网格的行高

```css
grid-auto-rows: 100px;
grid-auto-flow: column;
```

![](https://gitee.com/itsandy/picgo-img/raw/master/css/gird-auto-rows.png)

### 1.16 grid-auto-columns

[grid-auto-columns](https://developer.mozilla.org/zh-CN/docs/Web/CSS/grid-auto-columns) 控制隐式网格的列宽

```css
grid-auto-flow: column;
grid-auto-columns: 100px;
```

![](https://gitee.com/itsandy/picgo-img/raw/master/css/grid-auto-columns.png)

## 二、grid 子项

![](https://gitee.com/itsandy/picgo-img/raw/master/css/grid子项.png)

### 2.1 grid-row-start

[grid-row-start](https://developer.mozilla.org/en-US/docs/Web/CSS/grid-row-start) 控制 item 行的起始位置

### 2.2 grid-row-end

[grid-row-end](https://developer.mozilla.org/en-US/docs/Web/CSS/grid-row-end) 控制 item 行的结束位置

### 2.3 grid-row

[grid-row](https://developer.mozilla.org/zh-CN/docs/Web/CSS/grid-row) 控制 item 行的起始位置和结束位置的简写

### 2.4 grid-column-start

[grid-column-start](https://developer.mozilla.org/en-US/docs/Web/CSS/grid-column-start) 控制 item 列的起始位置

### 2.5 grid-column-end

[grid-column-end](https://developer.mozilla.org/en-US/docs/Web/CSS/grid-column-end) 控制 item 列的结束位置

### 2.6 grid-column

[grid-column](https://developer.mozilla.org/en-US/docs/Web/CSS/grid-column) 控制 item 列起始位置和结束位置的简写

其实在线中还存在一个`span`,这个值表示占据的**个数**

```css
grid-template-rows: [col1] 1fr [col2] 1fr [col3] 1fr;
```

父容器行宽和列高定义了变量在 item 的 row 和 column 可以使用对应的变量来进行映射

```css
.box div:nth-child(1) {
  background-color: gray;
  grid-row-start: col1;
  grid-row-end: span 3;
  grid-column: 2/3;
}
```

![](https://gitee.com/itsandy/picgo-img/raw/master/css/grid-column.png)

### 2.7 grid-area

[grid-area](https://developer.mozilla.org/zh-CN/docs/Web/CSS/grid-area)是控制 item 行列起始和结束位置的简写

```css
grid-area: 2/2/3/3;
```

如果指定了 4 个值，grid-row-start 会被设为第一个值，grid-column-start 为第二个值， grid-row-end 为第三个值，grid-column-end 为第四个值。

![](https://gitee.com/itsandy/picgo-img/raw/master/css/grid-area基本特性.png)

也可以指定当前 item 放在那个区域**额外特性**,可以指定自定义的 name 或者是`坐标`名称

```css
grid-template-areas: "a b c";
```

```css
.box div:nth-child(1) {
  background-color: gray;
  grid-area: b;
}

.box div:nth-child(2) {
  background-color: orange;
  grid-area: 1/3;
}

.box div:nth-child(3) {
  background-color: pink;
  grid-area: a;
}
```

![](https://gitee.com/itsandy/picgo-img/raw/master/css/grid-area.png)

默认会按照网格自动填充,指定的位置并不会影响到其它的单元格,其它的单元格会自动往其它位置进行填充

### 2.8 justify-self

[justify-self](https://developer.mozilla.org/zh-CN/docs/Web/CSS/justify-self) 控制具体 item 的水平对齐方式

### 2.9 align-self

[align-self](https://developer.mozilla.org/zh-CN/docs/Web/CSS/align-self) 控制具体 item 的垂直对齐方式

?>控制对齐方式 item 必须有宽度和高度

```css
.box div:nth-child(1) {
  background-color: gray;
  width: 50px;
  height: 50px;
  justify-self: center;
  align-self: center;
}
```

![](https://gitee.com/itsandy/picgo-img/raw/master/css/justify-self.png)

### 2.10 place-self

[place-self](https://developer.mozilla.org/en-US/docs/Web/CSS/place-self) 控制具体 item 的水平垂直对齐的简写

```css
place-self: center end;
```

![](https://gitee.com/itsandy/picgo-img/raw/master/css/place-self.png)

## 三、repeat 和 minmax

### 3.1 repeat

[repeat](<https://developer.mozilla.org/zh-CN/docs/Web/CSS/repeat()>)翻译就是重复

在我们写行和列的时候可以使用 repeat 替换到**重复且紧凑**的数值

```css
grid-template-columns: 100px 100px 100px;
```

等价于

```css
grid-template-columns: repeat(3, 100px);
```

这里我们指定了三列,当我们有 4 列的时候,新的一列就会变成换到下一行

那么我们可以使用 auto-fill,它会自动根据父容器的大小来进行调整的

```css
width: 400px;
grid-template-columns: repeat(auto-fill, 100px);
```

前提条件是你的父容器是否还剩可用的空间能够让够让你自适应

比如 width 为 300px,我原来只有三列,我多加了一列设置了 grid-template-columns: repeat(3, 100px),我想第 4 个也在第一列,那是不行的

父容器已经没有可用的空间了,所以想让第 4 个也在第一列,我们就得把父容器的 width 设为 400px

![](https://gitee.com/itsandy/picgo-img/raw/master/css/repeat.png)

### 3.2 minmax

[minmax](<https://developer.mozilla.org/zh-CN/docs/Web/CSS/minmax()>) 设置最小和最大的范围

我们先来看一个案例

```css
width: 0px;
grid-template-columns: 100px 1fr 100px;
```

![](https://gitee.com/itsandy/picgo-img/raw/master/css/自适应网格.gif)

我们会发现当我们父容器没有宽度,2 设置了对应的比例,2 就会进行自适应,最大可以一直放大,最小只能到内容的时候

但是如果我们想让 2 也是自适应,给它设置一个最小值,我们就可以使用 minmax 属性

```css
grid-template-columns: 100px minmax(100px, 1fr) 100px;
```

![](https://gitee.com/itsandy/picgo-img/raw/master/css/minmax.gif)

### 3.3 repeat 和 minmax 结合

repeat 和 minmax 结合起来做自适应的布局非常得简单且爽

```css
grid-template-columns: repeat(auto-fill, minmax(100px, 1fr));
grid-template-rows: 100px;
grid-auto-rows: 100px;
grid-gap: 20px 20px;
```

![](https://gitee.com/itsandy/picgo-img/raw/master/css/完美自适应.gif)

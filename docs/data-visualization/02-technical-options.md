# 数据可视化前端技术选型

前端数据可视化解决方案如下：

![](https://gitee.com/itsandy/picgo-img/raw/master/数据可视化/数据可视化解决方案.png)

## 一、底层

### 1.1 Skia

[Skia](https://github.com/google/skia) 是 Chrome 和 Android 的底层 2D 绘图引擎，具体可参考[维基百科](https://zh.wikipedia.org/zh-hans/Skia_Graphics_Library)，Skia 采用 C++ 编程，由于它位于浏览器的更底层，所以我们平常接触较少

!>对底层绘图感兴趣的可以从这个[案例](http://www.kevinbeason.com/smallpt/)入手，了解一下 C++ 的可视化编程。

### 1.2 OpenGL

[OpenGL](https://zh.wikipedia.org/zh/OpenGL)（Open Graphics Library）是 2D、3D 图形渲染库，它可以绘制从简单的 2D 图形到复杂的 3D 景象。OpenGL 常用于 CAD、VR、数据可视化和游戏等众多领域。

## 二、实现方式

### 2.1 HTML

[HTML](https://developer.mozilla.org/zh-CN/docs/Web/HTML)（超文本标记语言——HyperText Markup Language）是构成 Web 世界的一砖一瓦。它定义了网页内容的含义和结构。除 HTML 以外的其它技术则通常用来描述一个网页的表现与展示效果（如 CSS），或功能与行为（如 JavaScript）。

### 2.2 WebGL

[WebGL](https://zh.wikipedia.org/wiki/WebGL)（Web Graphics Library）是一种 3D 绘图协议，WebGL 可以为 HTML5 Canvas 提供硬件 3D 加速渲染，这样 Web 开发人员就可以借助系统显卡来在浏览器里更流畅地展示 3D 场景和模型了，还能创建复杂的导航和数据视觉化。

WebGL 案例分享

- [3D 魔方](http://www.randelshofer.ch/webgl/rubikscube/)
- [化学模型](https://web.chemdoodle.com/demos/molgrabber-3d)
- [3D 地球](http://www.webglearth.com/)
- [3D 大脑](https://www.biodigital.com/)

### 2.3 Svg

[SVG](https://developer.mozilla.org/zh-CN/docs/Web/SVG)是一种基于 XML 的图像文件格式，它的英文全称为 Scalable Vector Graphics，意思为可缩放的矢量图形

[入门案例：绘制点、矩形、直线和圆形](https://codesandbox.io/embed/svg-forked-6r8qn?fontsize=14&hidenavigation=1&theme=dark)

### 2.4 canvas

[canvas](https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API) 是 HTML5 的新特性，它允许我们使用 canvas 元素在网页上通过 JavaScript 绘制图像。

[入门案例：绘制点、矩形、直线和圆形](https://codesandbox.io/embed/canvas-f172g?fontsize=14&hidenavigation=1&theme=dark)

## 三、库

### 3.1 Three.js

[Three.js](https://threejs.org/) 是一个基于 WebGL 的 Javascript 3D 图形库

[官方案例:旋转正方体](https://codesandbox.io/embed/three-js-forked-r939m?fontsize=14&hidenavigation=1&theme=dark)

### 3.2 zrender

[zrender](https://ecomfe.github.io/zrender-doc/public/) 是二维绘图引擎，它提供 Canvas、SVG、VML 等多种渲染方式。ZRender 也是 ECharts 的渲染器

[入门案例：绘制点、矩形、直线和圆形](https://codesandbox.io/embed/zrender-phxpf?fontsize=14&hidenavigation=1&theme=dark)

### 3.3 D3

[D3](https://d3js.org/)（Data-Driven Documents） 是一个 Javascript 图形库，基于 Canvas、Svg 和 HTML。

[D3 的案例](https://observablehq.com/@d3/gallery)

!> D3 是一个较为复杂的图形库，如果想入门 D3 可以从 [这里](https://zhuanlan.zhihu.com/p/38001672)开始

## 四、框架

### 4.1 Highcharts

[Highcharts](https://www.highcharts.com/) 是一个用纯 JavaScript 编写的一个图表库， 能够很简单便捷的在 web 网站或是 web 应用程序添加有交互性的图表，并且免费提供给个人学习、个人网站和非商业用途使用。Highcharts 系列包含 Highcharts JS，Highstock JS，Highmaps JS 共三款软件，均为纯 JavaScript 编写的 HTML5 图表库。

[案例：Highcharts 折线图](https://codesandbox.io/embed/highcharts-kdxj4?fontsize=14&hidenavigation=1&theme=dark)

[Highstock](https://www.highcharts.com/demo/stock) 是用纯 JavaScript 编写的股票图表控件，可以开发股票走势或大数据量的时间轴图表。它包含多个高级导航组件：预设置数据时间范围，日期选择器、滚动条、平移、缩放功能。

[案例：平安银行股价 K 线图](https://codesandbox.io/embed/highstock-60p4f?fontsize=14&hidenavigation=1&theme=dark)

[Highmaps](https://www.highcharts.com/demo/maps) 是一款基于 HTML5 的优秀地图组件。Highmaps 继承了 Highcharts 简单易用的特性，利用它可以方便快捷的创建用于展现销售、选举结果等其他与地理位置关系密切的交互性地图图表。

[案例：欧洲时区](https://codesandbox.io/embed/highmaps-zjh4h?fontsize=14&hidenavigation=1&theme=dark)

### 4.2 AntV

[AntV](https://antv.vision/zh/) 是蚂蚁金服全新一代数据可视化解决方案，致力于提供一套简单方便、专业可靠、无限可能的数据可视化最佳实践。

AntV 包括以下解决方案：

- G2：可视化引擎
- G2Plot：图表库
- G6：图可视化引擎
- Graphin：基于 G6 的图分析组件
- F2：移动可视化方案
- ChartCube：AntV 图表在线制作
- L7：地理空间数据可视化

[L7 案例：气泡图](https://codesandbox.io/embed/l7-an-li-qi-pao-tu-r8yg9?fontsize=14&hidenavigation=1&theme=dark)

### 4.3 ECharts

[Apache ECharts](https://echarts.apache.org/zh/index.html)是一个基于 JavaScript 的开源可视化图表库

[入门案例：销售柱状图](https://codesandbox.io/embed/echartsru-men-an-li-wwfkn?fontsize=14&hidenavigation=1&theme=dark)

ECharts 也可以进行[主题配置](https://echarts.apache.org/zh/theme-builder.html)也可以使用这个主题编辑器，自己编辑主题。下载下来的主题可以这样使用：

## 五、ECharts 基本概念

### 5.1 系列

系列（series）是指：一组数值映射成对应的图

![](https://gitee.com/itsandy/picgo-img/raw/master/数据可视化/ECharts系列.png)

[案例：多系列混合](https://codesandbox.io/embed/echartsxi-lie-g28eu?fontsize=14&hidenavigation=1&theme=dark)

### 5.2 数据集

ECharts 4 开始支持了 [数据集](https://echarts.apache.org/handbook/zh/concepts/dataset)（dataset）组件用于单独的数据集声明，从而数据可以单独管理，被多个组件复用，并且可以自由指定数据到视觉的映射。这一特性能将逻辑和数据分离，带来更好的复用，并易于理解。

![](https://gitee.com/itsandy/picgo-img/raw/master/数据可视化/ECharts数据集.png)

[案例：dataset 移植](https://codesandbox.io/embed/echartsshu-ju-ji-lwb49?fontsize=14&hidenavigation=1&theme=dark)

### 5.3 组件
## 代码片段

我们学习 vue 或者是 react 的时候，有些代码片段是需要经常写的，每次创建一个文件我们就需要一个基本的模板，然后我们手动敲的花，太傻了，当然你也可以借助一些插件来生成代码片段也是可以的，这里我也有一种方法可以快速生成代码片段

这里我就以 vscode 为代码，来进行演示(vscode 天下第一 🍖)

接下来我就简单来演示一下步骤

比如我想生成这样的一个模板

```vue
<template>
  <div></div>
</template>

<script>
export default {};
</script>

<style lang="scss" scoped></style>
```

复制代码,然后打开[https://snippet-generator.app/](https://snippet-generator.app/)

具体看图：

![image.png](https://img14.360buyimg.com/ddimg/jfs/t1/181856/18/18716/76857/61137cf8E34f3aed7/c202eaaa529ddf4e.png)

拷贝成功以后在 vscode 中配置代码片段,详情看图：

![image.png](https://img10.360buyimg.com/ddimg/jfs/t1/200348/3/2588/26280/61137f19E300967f2/604e82d22a7f81e0.png)
![image.png](https://img10.360buyimg.com/ddimg/jfs/t1/180507/17/18603/28314/61137edeE4ddd6c3f/2e2563bf43f689d6.png)

如果是vue的话,就是vue.json,如果是react的话就是javascript.json(js)或者javascriptreact.json(jsx)或者typescriptreact.json(tsx)

```json
{
  "v2": {
    "prefix": "v2",
    "body": [
      "<template>",
      "  <div>",
      "",
      "  </div>",
      "</template>",
      "",
      "<script>",
      "  export default {",
      "    ",
      "  }",
      "</script>",
      "",
      "<style lang=\"scss\" scoped>",
      "",
      "</style>"
    ],
    "description": "v2"
  }
}
```

配置完成之后输入你配置的`prefix`的值以后，整个模板就会快速生成


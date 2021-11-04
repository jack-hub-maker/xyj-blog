## 复杂 data 的处理方式

我们知道，在模板中可以直接通过插值语法显示一些 data 中的数据。

但是在某些情况，我们可能需要对数据进行一些转化后再显示，或者需要将多个数据结合起来进行显示；

- 比如我们需要对`多个 data 数据进行运算、三元运算符来决定结果、数据进行某种转化`后显示；
- 在模板中使用`表达式`，可以非常方便的实现，但是设计它们的初衷是用于`简单的运算`；
- 在模板中放入太多的逻辑会让`模板过重和难以维护`；
- 并且如果多个地方都使用到，那么会有大量重复的代码；

我们有没有什么方法可以将逻辑抽离出去呢？

- 可以，其中一种方式就是将逻辑抽取到一个 `method` 中，放到 methods 的 options 中；
- 但是，这种做法有一个直观的弊端，就是所有的 data 使用过程都会变成了一个`方法的调用`；
- 另外一种方式就是使用计算属性 `computed`；

## 计算属性 computed

什么是计算属性呢？

- 官方并没有给出直接的概念解释；
- 而是说：对于任何包含响应式数据的复杂逻辑，你都应该使用**计算属性**；
- 计算属性将被混入到组件实例中。所有 getter 和 setter 的 this 上下文自动地绑定为组件实例；

## 案例实现思路

我们来看三个案例：

案例一：我们有两个变量：firstName 和 lastName，希望它们拼接之后在界面上显示；

案例二：我们有一个分数：score，当 score 大于 60 的时候，在界面上显示及格；当 score 小于 60 的时候，在界面上显示不及格；

案例三：我们有一个变量 message，记录一段文字：比如 Hello World，某些情况下我们是直接显示这段文字；某些情况下我们需要对这段文字进行反转；

我们可以有三种实现思路：

- 在模板语法中直接使用表达式；
- 使用 method 对逻辑进行抽取；
- 使用计算属性 computed；

## 实现思路一：模板语法

思路一的实现：模板语法

- 缺点一：模板中存在大量的复杂逻辑，不便于维护（模板中表达式的初衷是用于简单的计算）；
- 缺点二：当有多次一样的逻辑时，存在重复的代码；
- 缺点三：多次使用的时候，很多运算也需要多次执行，没有缓存；

```vue
<template>
  <div>
    <input type="text" v-model="score" />
    <div>{{ firstName + lastName }}</div>
    <div v-if="score">及格</div>
    <div v-else>不及格</div>
    <div>{{ message.split("").reverse().join("") }}</div>
  </div>
</template>
```

```js
<script>
export default {
  data() {
    return {
      firstName: "rk",
      lastNam0e: "md",
      score: 0,
      message: "Hello World",
    };
  }
};
</script>
```

## 实现思路二：method 实现

思路二的实现：method 实现

- 缺点一：我们事实上先显示的是一个结果，但是都变成了一种方法的调用；
- 缺点二：多次使用方法的时候，没有缓存，也需要多次计算（也没有代码提示）

```vue
<template>
  <div>
    <input type="text" v-model="score" />
    <div>{{ getFullName() }}</div>
    <div>{{ getResult() }}</div>
    <div>{{ getReverseMessage() }}</div>
  </div>
</template>
```

```js
<script>
export default {
  data() {
    return {
      firstName: "rk",
      lastNam0e: "md",
      score: 0,
      message: "Hello World",
    };
  },
  methods: {
    getFullName() {
      return this.firstName + "" + this.lastNam0e;
    },
    getResult() {
      return this.score > 60 ? "及格" : "不及格";
    },
    getReverseMessage() {
      return this.message.split("").reverse().join("");
    },
  },
};
</script>
```

## 思路三的实现：computed 实现

思路三的实现：computed 实现

- 注意：计算属性看起来像是一个函数，但是我们在使用的时候不需要加()，这个后面讲 setter 和 getter 时会讲到；
- 我们会发现无论是直观上，还是效果上计算属性都是更好的选择；
- 并且计算属性是有缓存的；

```vue
<template>
  <div>
    <input type="text" v-model="score" />
    <div>{{ getFullName }}</div>
    <div>{{ getResult }}</div>
    <div>{{ getReverseMessage }}</div>
  </div>
</template>
```

```js
<script>
export default {
  data() {
    return {
      firstName: "rk",
      lastNam0e: "md",
      score: 0,
      message: "Hello World",
    };
  },
  computed: {
    getFullName() {
      return this.firstName + "" + this.lastNam0e;
    },
    getResult() {
      return this.score > 60 ? "及格" : "不及格";
    },
    getReverseMessage() {
      return this.message.split("").reverse().join("");
    },
  },
};
</script>

```

## 计算属性 vs methods

在上面的实现思路中，我们会发现计算属性和 methods 的实现看起来是差别是不大的，而且我们多次提到计算属
性**有缓存**的。

接下来我们来看一下同一个计算多次使用，计算属性和 methods 的差异：

```vue
<template>
  <div>
    <h2>{{ getFullName() }}</h2>
    <h2>{{ getFullName() }}</h2>
    <h2>{{ getFullName() }}</h2>
    <h2>{{ fullName }}</h2>
    <h2>{{ fullName }}</h2>
    <h2>{{ fullName }}</h2>
  </div>
</template>
```

```js
<script>
export default {
  data() {
    return {
      firstName: "rk",
      lastNam0e: "md",
    };
  },
  methods: {
    getFullName() {
      console.log("调用了getFullName函数");
      return this.firstName + "" + this.lastNam0e;
    },
  },
  computed: {
    fullName() {
      console.log("调用了fullName计算属性");
      return this.firstName + "" + this.lastNam0e;
    },
  },
};
</script>
```

![image.png](https://img11.360buyimg.com/ddimg/jfs/t1/61972/8/17118/6176/613f353cE5dd932d1/450609293e393e5d.png)

## 计算属性的缓存

这是什么原因呢？

- 这是因为计算属性会基于它们的`依赖关系进行缓存`；
- 在`数据不发生变化`时，计算属性是`不需要重新计算`的；
- 但是如果`依赖的数据发生变化`，在使用时，计算属性依然`会重新进行计算`；

## 计算属性的 setter 和 getter

计算属性在大多数情况下，只需要一个 getter 方法即可，所以我们会将计算属性直接写成一个函数。

但是，如果我们确实想设置计算属性的值呢？

这个时候我们也可以给计算属性设置一个`setter的方法`；

```vue
<template>
  <div>
    <button @click="changeName">修改name</button>
    <h2>{{ fullName }}</h2>
  </div>
</template>
```

```js
<script>
export default {
  data() {
    return {
      firstName: "rk",
      lastNam0e: "md",
    };
  },
  methods: {
    changeName() {
      this.fullName = "tao";
    },
  },
  computed: {
    fullName: {
      get() {
        return this.firstName + "" + this.lastNam0e;
      },
      set(newValue) {
        console.log(newValue);
        const names = newValue.split("");
        this.firstName = names[0];
        this.lastNam0e = names[1];
      },
    },
  },
};
</script>
```

## 源码如何对 setter 和 getter 处理呢？

你可能觉得很奇怪，Vue 内部是如何对我们传入的是一个 getter，还是说是一个包含 setter 和 getter 的对象进行处
理的呢？

事实上非常的简单，Vue 源码内部只是做了一个逻辑判断而已；

![image.png](https://img14.360buyimg.com/ddimg/jfs/t1/84826/14/18964/330816/613f41e0E6f0ba753/e771c09bb9f081a6.png)

## 认识侦听器 watch

什么是侦听器呢？

- 开发中我们在 data 返回的对象中定义了数据，这个数据通过`插值语法等方式绑定到template`中；
- 当数据变化时，template 会自动进行更新来显示最新的数据；
- 但是在某些情况下，我们希望在`代码逻辑`中监听某个数据的变化，这个时候就需要用`侦听器watch`来完成了；

## 侦听器案例

比如现在我们希望用户在`input中输入一个问题`；

每当用户输入了最新的内容，我们就获取到最新的内容，并且`使用该问题去服务器查询答案`；

那么，我们就需要实时的去获取最新的数据变化；

```vue
<template>
  <div>
    <input type="text" v-model="message" />
  </div>
</template>
```

```js
<script>
export default {
  data() {
    return {
      message: "",
    };
  },
  methods: {
    gain(newValue) {
      console.log(`${newValue}的答案是哈哈哈`);
    },
  },
  watch: {
    message(newValue, oldValue) {
      console.log(oldValue);
      this.gain(newValue);
    },
  },
};
</script>
```

## 侦听器 watch 的配置选项

我们先来看一个例子：

- 当我们点击按钮的时候会修改`info.name`的值；
- 这个时候我们使用`watch来侦听info，可以侦听到吗`？答案是`不可以`。

这是因为默认情况下，watch 只是在侦听 info 的引用变化，对于内部属性的变化是不会做出响应的：

- 这个时候我们可以使用一个`选项deep`进行更深层的侦听；
- 注意前面我们说过 watch 里面侦听的属性对应的也可以是一个 Objec；

还有另外一个属性，是希望一开始的就会立即执行一次：

- 这个时候我们使用`immediate选项`；
- 这个时候无论后面数据是否有变化，侦听的函数都会有限执行一次；

```vue
<template>
  <div>
    <div>{{ info.name }}</div>
    <button @click="changeInfo">按钮</button>
    <button @click="changeInfoName">按钮</button>
  </div>
</template>
```

```js
<script>
export default {
  data() {
    return {
      info: {
        name: "tao",
        age: 19,
      },
    };
  },
  methods: {
    changeInfo() {
      this.info = { name: "sandy" };
    },
    changeInfoName() {
      this.info.name = "sandy";
    },
  },
  watch: {
    // 默认情况下我们的侦听器只会针对监听的数据本身的改变（内部发生的改变是不能侦听）
    // info(newValue, oldValue) {
    //   console.log("oldValue", oldValue, "newValue", newValue);
    // },

    info: {
      handler(newInfo, oldInfo) {
        console.log("newInfo", newInfo, "oldInfo", oldInfo);
      },
      // 深度侦听
      deep: true,
      // 立即执行
      immediate: true,
    },
  },
};
</script>
```

如果你想直接侦听 info.name，也有一种方法，这个是 Vue3 文档中没有提到的，但是 Vue2 文档中有提到的是侦听对象的属性：

```js
  "info.name": function (newValue, oldValue) {
    console.log(newValue, oldValue);
  },
```

如果你想侦听变化，调用多个函数，你也可以传入一个数组

这个开发中用的还是比较少的

还有另外一种方式就是使用 $watch 的 API：

我们可以在 created 的生命周期（后续会讲到）中，使用 this.$watchs 来侦听；

- 第一个参数是要侦听的源；
- 第二个参数是侦听的回调函数 callback；
- 第三个参数是额外的其他选项，比如 deep、immediate；

```js
  created() {
    this.$watch(
      "info",
      (newValue, oldValue) => {
        console.log(newValue, oldValue);
      },
      {
        deep: true,
      }
    );
  },
```


!>一般来说我们在开发中需要用到侦听器都是比较简单的场景，如果遇到复杂的场景可以仔细查阅文档

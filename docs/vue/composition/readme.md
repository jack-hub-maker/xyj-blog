# Composition API

## 一、Options API 的弊端

在 Vue2 中，我们编写组件的方式是 Options API：

- Options API 的一大特点就是在**对应的属性**中编写**对应的功能模块**；
- 比如 data 定义数据、methods 中定义方法、computed 中定义计算属性、watch 中监听属性改变，也包括生命
  周期钩子；

但是这种代码有一个很大的弊端：

- 当我们实现某一个功能时，这个功能对应的代码逻辑会被拆分到各个属性中；
- 当我们组件变得更大、更复杂时，逻辑关注点的列表就会增长，那么同一个功能的逻辑就会被拆分的很分散；
- 尤其对于那些一开始没有编写这些组件的人来说，这个组件的代码是难以阅读和理解的（阅读组件的其他人）；

下面我们来看一个非常大的组件，其中的逻辑功能按照颜色进行了划分：

- 这种碎片化的代码使用理解和维护这个复杂的组件变得异常困难，并且隐藏了潜在的逻辑问题；
- 并且当我们处理单个逻辑关注点时，需要不断的跳到相应的代码块中；

## 二、认识 Composition API

如果我们能将同一个逻辑关注
点相关的代码收集在一起会更
好。

这就是 Composition API 想
要做的事情，以及可以帮助我
们完成的事情。

也有人把 Vue Composition
API 简称为`VCA`。

那么既然知道 Composition API 想要帮助我们做什么事情，接下来看一下到底是怎么做呢？

- 为了开始使用 Composition API，我们需要有一个可以实际使用它（编写代码）的地方；
- 在 Vue 组件中，这个位置就是 `setup 函数`；

setup 其实就是组件的另外一个选项：

- 只不过这个选项强大到我们可以用它来替代之前所编写的大部分其他选项；
- 比如 methods、computed、watch、data、生命周期等等；

我们直接通过一个计数器的案例来看看 Options API 和 Composition API

```js
<script>
export default {
  data() {
    return {
      count: 0,
    };
  },
  methods: {
    increment() {
      this.count++;
    },
    decrement() {
      this.count--;
    },
  },
};
</script>
```

```js
import { ref } from "vue";
export default {
  setup() {
    const count = ref(0);
    const increment = () => {
      count.value++;
    };
    const decrement = () => {
      count.value--;
    };
    return {
      count,
      increment,
      decrement,
    };
  },
};
```

诶？你这怎么比 v2 的代码还多啊，感觉不怎么样啊！

不急，下面听我慢慢讲解，看完了以后你肯定会爱不释手

## 三、setup 函数

### 3.1 setup 函数的参数

我们先来研究一个 setup 函数的参数，它主要有两个参数：

- 第一个参数：props
- 第二个参数：context

props 非常好理解，它其实就是父组件传递过来的属性会被放到 props 对象中，我们在 setup 中如果需要使用，那么就可
以直接通过 props 参数获取：

- 对于定义 props 的类型，我们还是和之前的规则是一样的，在 props 选项中定义；
- 并且在 template 中依然是可以正常去使用 props 中的属性
- 如果我们在 setup 函数中想要使用 props，那么不可以通过 this 去获取
- 因为 props 有直接作为参数传递到 setup 函数中，所以我们可以直接通过参数来使用即可；

另外一个参数是 context，我们也称之为是一个 SetupContext，它里面包含三个属性：

- attrs：所有的非 prop 的 attribute；
- slots：父组件传递过来的插槽
- emit：当我们组件内部需要发出事件时会用到 emit

```js
setup(props, context) {
  console.log(props);
  console.log(context.attrs)
  console.log(context.slots)
  console.log(context.emit)
},
```

### 3.2 setup 函数的返回值

setup 既然是一个函数，那么它也可以有返回值，它的返回值用来做什么呢？

- setup 的返回值可以在**模板 template 中被使用**；
- 也就是说我们可以**通过 setup 的返回值来替代 data 选项**；

甚至是我们可以返回一个执行函数来代替在 methods 中定义的方法：

```js
setup() {
  let count = 0;
  const increment = () => {
    count++
  }
  const decrement = () => {
    count--
  }
  return {
    count,
    increment,
    decrement
  };
},
```

但是，如果我们将 counter 在 increment 或者 decrement 进行操作时，是否可以实现界面的响应式呢？

- 答案是**不可以**；
- 这是因为对于一个**定义的变量**来说，默认情况下，**Vue 并不会跟踪它的变化，来引起界面的响应式操作**；

### 3.3 setup 不可以使用 this

官方关于 this 有这样一段[描述](https://v3.cn.vuejs.org/guide/composition-api-introduction.html#setup-%E7%BB%84%E4%BB%B6%E9%80%89%E9%A1%B9)

- 表达的含义是**this 并没有指向当前组件实例**；
- 并且**在 setup 被调用之前，data、computed、methods**等都没有被解析；
- 所以**无法在 setup 中获取 this**；

### 3.4 setup 语法糖

`script setup`也是 v3 让开发者体验更爽的一个比较好的更新

要使用这个语法，需要将 setup attribute 添加到 script 代码块上：

```js
<script setup></script>
```

使用 setup 语法糖之后，我们也需要 return 一个对象了，它会自动帮我们返回给 template 进行使用

在 `script setup `中必须使用 `defineProps` 和 `defineEmits` API 来声明 props 和 emits ，它们具备完整的类型推断并且在 `script setup `中是直接可用的：

```js
<script setup>
const props = defineProps({
  foo: String
})

const emit = defineEmits(['change', 'delete'])
// setup code
</script>
```

还有一个比较好的更新是，使用 setup 语法糖以后，我们导入组件不再需要在 components 里面进行添加了

更多细节请点击[详情](https://v3.cn.vuejs.org/api/sfc-script-setup.html#%E5%8D%95%E6%96%87%E4%BB%B6%E7%BB%84%E4%BB%B6-script-setup)

## 四、Ref API

### 4.1 ref

如果想让 setup 中定义的数据具有响应式的特性，那么我们可以使用[ref](https://v3.cn.vuejs.org/api/refs-api.html#ref)函数

ref 会返回一个可变的响应式对象，该对象作为一个 响应式的引用 维护着它内部的值，这就是 ref 名称的来源(**引用对象**)

它内部的值是在 ref 的 value 属性中被维护的；

```js
<script setup>import {ref} from "vue"; const count = ref(0);</script>
```

这里有两个注意事项：

- 在模板中引入 ref 的值时，Vue 会**自动帮助我们进行解包操作**，所以我们并**不需要在模板中通过 ref.value 的方式**
  来使用；
- 但是在 setup 函数内部，它依然是一个 ref 引用， 所以对其进行操作时，我们依然需要使用 ref.value 的方式；

```html
<template>
  <div>
    <h2>{{ count }}</h2>
    <button @click="increment">+1</button>
  </div>
</template>

<script setup>
  import { ref } from "vue";
  const count = ref(0);
  const increment = () => {
    count.value++;
  };
</script>
```

ref 接收的数据可以是基本数据类型也可以是对象类型

基本数据类型的数据：响应式依然是靠`Object.defineProperty()`的`get`与`set`完成的

对象类型的数据：内部依靠的是 v3 中的一个新函数`reactive`函数（把放入的数据加工成了一个 Proxy 对象，具体 Proxy 的操作是在 reactive 函数里进行实现的）

```html
<template>
  <div>
    <h2>{{ info.count }}</h2>
    <button @click="increment">+1</button>
  </div>
</template>

<script setup>
  import { ref } from "vue";
  const info = ref({
    count: 0,
  });
  const increment = () => {
    info.value.count++;
  };
</script>
```

### 4.2 shallowRef

[shallowReadonly](https://v3.cn.vuejs.org/api/refs-api.html#shallowref)是**浅层的**

```html
<template>
  <div>
    <h2>{{ counter.info.count }}</h2>
    <button @click="counter.info.count++">+1</button>
  </div>
</template>

<script setup>
  import { shallowRef } from "vue";
  const counter = shallowRef({
    info: {
      count: 0,
    },
  });
</script>
```

这样点击按钮是不会有响应式的

## 五、Reactive API

### 5.1 reactive

[reactive](https://v3.cn.vuejs.org/api/basic-reactivity.html#reactive)函数定义一个对象类型的响应式数据，返回一个代理对象（proxy 的实例对象）

```html
<template>
  <div>
    <h2>{{ counter.count }}</h2>
    <button @click="increment">+1</button>
  </div>
</template>

<script setup>
  import { reactive } from "vue";
  const counter = reactive({
    count: 0,
  });
  const increment = () => {
    counter.count++;
  };
</script>
```

reactive 定义的响应式数据是**深层次**的

内部基于 ES6 的 proxy 进行实现的，通过代理对象操作源对象内部数据进行操作

```html
<template>
  <div>
    <h2>{{ counter.count }}</h2>
    <h2>{{ counter.a.b.c }}</h2>
    <button @click="increment">+1</button>
  </div>
</template>

<script setup>
  import { reactive } from "vue";
  const counter = reactive({
    count: 0,
    a: {
      b: {
        c: "ccc",
      },
    },
  });
  const increment = () => {
    counter.count++;
    counter.a.b.c = "eee";
  };
</script>
```

### 5.2 shallowReactive

shallowReactive 是**浅层的**

```html
<template>
  <div>
    <h2>{{ counter.info.count }}</h2>
    <button @click="counter.info.count++">+1</button>
  </div>
</template>

<script setup>
  import { shallowReactive } from "vue";
  const counter = shallowReactive({
    info: {
      count: 0,
    },
  });
</script>
```

## 六、readonly API

### 6.1 readonly

我们通过 reactive 或者 ref 可以获取到一个响应式的对象，但是某些情况下，我们传入给其他地方（组件）的这个
响应式对象希望在另外一个地方（组件）被使用，但是不能被修改，这个时候如何防止这种情况的出现呢？

- Vue3 为我们提供了[readonly](https://v3.cn.vuejs.org/api/basic-reactivity.html#readonly)的方法；
- **readonly 会返回原生对象的只读代理**（也就是它依然是一个 Proxy，这是一个**proxy 的 set 方法被劫持**，并且不
  能对其进行修改）；

其实学过 v2 的话，第一眼就知道这是什么意思了，只读的，那肯定一般都是用在传递子组件的 props 咯（单向数据流）

v2 中如果开发者非要改 props 的值，也是没有办法的，有了 readonly 我们就可以编写对应的代码，来更好的来遵守单向数据流的规范

```html
<template>
  <div>
    <h2>{{ counter.info.a.b.count }}</h2>
    <button @click="counter.info.a.b.count++">+1</button>
    <Home :info="info" />
  </div>
</template>

<script setup>
  import { ref, readonly } from "vue";
  import Home from "./Home.vue";
  const counter = ref({
    name: "tao",
    info: {
      a: {
        b: {
          count: 0,
        },
      },
    },
  });
  const info = readonly(counter);
</script>
```

```html
<template>
  <div>
    <h2>{{ props.info.info.a.b.count }}</h2>
    <button @click="props.info.info.a.b.count++">+1</button>
  </div>
</template>

<script setup>
  const props = defineProps({
    info: {
      type: Object,
    },
  });
</script>
```

在 Home 组件中是无法修改 count 的值的，但是在 App 自己组件内部就可以修改，这样就可以让多人开发中更好的遵循单向数据流的规范

### 6.2 shallowReadonly

[shallowReadonly](https://v3.cn.vuejs.org/api/basic-reactivity.html#shallowreadonly)是**浅只读**的

上面的案例中 count 是在 info 的深层里定义的，可以对它进行限制

如果是 shallowReadonly 的话，只能限制第一层，深层的属性是无法进行限制的

```html x
<script setup>
  import { shallowReadonly } from "vue";
  const info = shallowReadonly(counter);
</script>
```

## 七、toRefs API

### 7.1 toRefs

如果我们使用 ES6 的解构语法，对 reactive 返回的对象进行解构获取值，那么之后无论是修改结构后的变量，还是修改 reactive
返回的 state 对象，**数据都不再是响应式**的：

```js
let { name, age } = reactive({
  name: "tao",
  age: 18,
});
```

那么有没有办法让我们解构出来的属性是响应式的呢？

- Vue 为我们提供了一个[toRefs](https://v3.cn.vuejs.org/api/refs-api.html#torefs)的函数，可以将 reactive 返回的对象中的属性都转成 ref；
- 那么我们再次进行结构出来的 name 和 age 本身都是 ref 的；

```js
import { reactive, toRefs } from "vue";
const info = reactive({
  name: "tao",
  age: 18,
});
let { name, age } = toRefs(info);
```

这种做法相当于已经在 info 和 name,age 之间建立了 链接，任何一个修改都会引起另外一个变化；

### 7.2 toRef

如果我们只希望转换一个 reactive 对象中的属性为 ref, 那么可以使用[toRef](https://v3.cn.vuejs.org/api/refs-api.html#torefs)的方法：

```js
import { toRef } from "vue";
const age = toRef(info, "age");
```

## 八、customRef

创建一个自定义的 ref，并对其依赖项跟踪和更新触发进行显示控制：

- 它需要一个工厂函数，该函数接受 track 和 trigger 函数作为参数；
- 并且应该返回一个带有 get 和 set 的对象；

这里我们来使用官方列举到的一个案例（通过[customRef](https://v3.cn.vuejs.org/api/refs-api.html#customref)实现防抖函数）

```html
<template>
  <div>
    <input type="text" v-model="message" />
    <h2>{{ message }}</h2>
  </div>
</template>

<script setup>
  import { ref } from "vue";
  import useDebounce from "./useDebounce";
  const message = useDebounce("", 2000, true);
</script>

<style lang="scss" scoped></style>
```

```js
import { customRef } from "vue";

export default function (value, delay, Immediately = false) {
  let timer = null;
  let invoke = false;
  return customRef((track, trigger) => {
    return {
      get() {
        track(); // 收集依赖
        return value;
      },
      set(newValue) {
        if (timer) clearTimeout(timer);
        // 第一次立即执行
        if (Immediately && !invoke) {
          value = newValue;
          trigger();
          invoke = true;
        }
        timer = setTimeout(() => {
          value = newValue;
          trigger(); // 更新触发
        }, delay);
      },
    };
  });
}
```

## 九、computed

在 Composition API 中，我们可以在 setup 函数中使用 [computed](https://v3.cn.vuejs.org/api/computed-watch-api.html#computed) 方法来编写一个计算属性；

如何使用 computed 呢？

- 接收一个 getter 函数，并为 getter 函数返回的值，返回一个不变的 ref 对象；
- 接收一个具有 get 和 set 的对象，返回一个可变的（可读写）ref 对象；

```js
import { computed, ref } from "vue";
const firstName = ref("Hello");
const lastName = ref("World");
const fullName = computed(() => firstName.value + " " + lastName.value);
```

```js
const fullName = computed({
  get: () => firstName.value + " " + lastName.value,
});
```

## 十、watchEffect

当侦听到某些响应式数据变化时，我们希望执行某些操作，这个时候可以使用 [watchEffect](https://v3.cn.vuejs.org/api/computed-watch-api.html#watcheffect)。

watchEffect 会自动收集响应式的依赖(回调函数中用到了什么响应式属性就会监听对应的响应式属性)

```js
<script setup>
import { ref, watchEffect } from "vue";
const count = ref(0);
watchEffect(() => {
  console.log("count", count.value);
});
</script>
```

首先，watchEffect 传入的函数会被立即执行一次，并且在执行的过程中会收集依赖；

其次，只有收集的依赖发生变化时，watchEffect 传入的函数才会再次执行；

如果在发生某些情况下，我们希望停止侦听，这个时候我们可以获取 watchEffect 的返回值函数，调用该函数即可。

```js
const stop = watchEffect(() => {
  console.log("count", count.value);
});
const btnClick = () => {
  count.value++;
  if (count.value >= 5) {
    stop();
  }
};
```

watchEffect 也是可以清除副作用的

什么是清除副作用呢？

- 比如在开发中我们需要在侦听函数中执行网络请求，但是在网络请求还没有达到的时候，我们停止了侦听器，
  或者侦听器侦听函数被再次执行了。
- 那么上一次的网络请求应该被取消掉，这个时候我们就可以清除上一次的副作用；

在我们给 watchEffect 传入的函数被回调时，其实可以获取到一个参数：onInvalidate

- 当**副作用即将重新执行** 或者 **侦听器被停止** 时会执行该函数传入的回调函数；
- 我们可以在传入的回调函数中，执行一些清楚工作；

```js
const stop = watchEffect((onInvalidate) => {
  console.log("count", count.value);
  const timer = setTimeout(() => {
    console.log("发送网络请求");
  }, 2000);
  onInvalidate(() => {
    // 在整个函数中清除额外的副作用
    // 在组件销毁的时候也会回调一次
    // 可以理解为侦听器再次执行的时候先执行这个函数（清除副作用），然后再执行onInvalidate外面的代码
    clearTimeout(timer);
  });
});
```

这里再说一个东西，在 setup 中如何使用 ref 或者元素或者组件？

其实非常简单，我们只需要定义一个 ref 对象，绑定到元素或者组件的 ref 属性上即可；

```js
const title = ref(null);
watchEffect(() => {
  console.log(title.value);
});
```

我们会发现打印结果打印了两次：

- 这是因为 setup 函数在执行时就会立即执行传入的副作用函数，这个时候 DOM 并没有挂载，所以打印为 null；
- 而当 DOM 挂载时，会给 title 的 ref 对象赋值新的值，副作用函数会再次执行，打印出来对应的元素；

如果我们希望在第一次的时候就打印出来对应的元素呢？

- 这个时候我们需要改变副作用函数的执行时机；
- 它的默认值是 pre，它会在元素 挂载 或者 更新 之前执行；
- 所以我们会先打印出来一个空的，当依赖的 title 发生改变时，就会再次执行一次，打印出元素；

我们可以设置副作用函数的执行时机（watchEffect 的第二个参数）

```js
watchEffect(
  () => {
    console.log(title.value);
  },
  { flush: "post" }
);
```

!> flush 选项还接受 sync，这将强制效果始终同步触发。然而，这是**低效**的，应该很少需要。

## 十一、watch

[watch](https://v3.cn.vuejs.org/api/computed-watch-api.html#watch)的 API 完全等同于组件 watch 选项的 Property：

- watch 需要侦听特定的数据源，并在回调函数中执行副作用；
- 默认情况下它是惰性的，只有当被侦听的源发生变化时才会执行回调；

与 watchEffect 的比较，watch 允许我们：

- 懒执行副作用（第一次不会直接执行）；
- 更具体的说明当哪些状态发生变化时，触发侦听器的执行；
- 访问侦听状态变化前后的值；

watch 侦听函数的数据源有两种类型：

- 一个 getter 函数：但是该 getter 函数必须引用可响应式的对象（比如 reactive 或者 ref）；
- 直接写入一个可响应式的对象，reactive 或者 ref（比较常用的是 ref）；

```js
const counter = reactive({
  count: 0,
});
const count = ref(0);
// 情况一：一个getter函数
// reactive对象
watch(
  () => counter.count,
  (newValue, oldValue) => {
    console.log("newValue", newValue, "oldValue", oldValue);
  }
);
// ref对象
watch(
  () => count,
  (newValue, oldValue) => {
    console.log("newValue", newValue, "oldValue", oldValue);
  }
);
```

```js
// 情况二:一个响应式对象
const counter = reactive({
  count: 0,
});
const count = ref(0);
// reactive对象
watch(counter, (newValue, oldValue) => {
  // 拿到的结果是一个proxy对象
  console.log("newValue", newValue, "oldValue", oldValue);
});
// ref对象
watch(count, (newValue, oldValue) => {
  console.log("newValue", newValue, "oldValue", oldValue);
});
```

侦听器还可以使用数组同时侦听多个源：

```js
const name = ref("tao");
const age = reactive({
  age: 18,
});
watch([name, age], ([newName, newAge], [oldName, oldAge]) => {
  console.log(newName, newAge, oldName, oldAge);
});
```

如果我们希望侦听一个深层的侦听，那么依然需要设置 deep 为 true：

```js
// reactive默认就是一个深度侦听
// ref的话就不是一个深度侦听，需要设置一个deep为true
// immediate的话就是第一次就执行（跟v2是一样的）
watch(
  info,
  (newValue, oldValue) => {
    console.log("newValue", newValue, "oldValue", oldValue);
  },
  { deep: true, immediate: true }
);
```

## 十二、生命周期钩子

我们前面说过 setup 可以用来替代 data 、 methods 、 computed 、watch 等等这些选项，也可以替代 生命周
期钩子。

那么 setup 中如何使用生命周期函数呢？

可以使用直接导入的 onX 函数注册生命周期钩子；

![](https://gitee.com/itsandy/picgo-img/raw/master/vue/composition/生命周期钩子.png)

setup 函数其实是在 beforcreate 之前就执行了

!> 对于以前我们在 beforcreate 和 created 生命周期中做的事情都可以在 setup 中做

## 十三、provide/inject

事实上我们之前还学习过 Provide 和 Inject，Composition API 也可以替代之前的 [Provide 和 Inject](https://v3.cn.vuejs.org/guide/composition-api-provide-inject.html#provide-inject) 的选项。

我们可以通过 provide 来提供数据：

可以通过 provide 方法来定义每个 Property；

provide 可以传入两个参数：

- name：提供的属性名称；
- value：提供的属性值；

在 后代组件 中可以通过 inject 来注入需要的属性和对应的值：

可以通过 inject 来注入需要的内容；

- 要 inject 的 property 的 name；
- 默认值；

```html title="App.vue"
<template>
  <div>
    <h2>{{ count }}</h2>
    <button @click="count++">+1</button>
    <Home />
  </div>
</template>

<script setup>
  import { provide, ref } from "vue";
  import Home from "./Home.vue";
  const count = ref(0);
  provide("count", count);
</script>
```

```html title="Home.vue"
<template>
  <div>
    <h2>{{ count }}</h2>
    <button @click="count++">+1</button>
  </div>
</template>

<script setup>
  import { inject } from "vue";
  const count = inject("count");
</script>
```

:::info
其实你会发现这并不是很好，我通过 provide 传递了数据，你接收的时候可以随便改我的数据，并不符合我们开发的规范（**单向数据流**）
:::

学了前面的知识你肯定会想到传递一个 readonly 或者只传递一个修改数据的方法

```js
const increment = () => count.value++;
provide("increment", increment);
```

```js
const count = ref(0);
const readonlyCount = readonly(count);
provide("count", readonlyCount);
```

## 十四、Teleport

在组件化开发中，我们封装一个组件 A，在另外一个组件 B 中使用：

- 那么组件 A 中 template 的元素，会被挂载到组件 B 中 template 的某个位置；
- 最终我们的应用程序会形成一颗 DOM 树结构；

但是某些情况下，我们希望组件不是挂载在这个组件树上的，可能是移动到 Vue app 之外的其他位置：

- 比如移动到 body 元素上，或者我们有其他的 div#app 之外的元素上；
- 这个时候我们就可以通过 teleport 来完成；

[Teleport](https://v3.cn.vuejs.org/guide/teleport.html#teleport)是什么呢？

- 它是一个**Vue 提供的内置组件**，类似于 react 的 Portals；
- teleport 翻译过来是心灵传输、远距离运输的意思；
  - 它有两个属性：
    - to：指定将其中的内容移动到的目标元素，可以使用选择器；
    - disabled：是否禁用 teleport 的功能；

首先你需要在 index.html 里添加一个 id 为 title 的标签

之后我们就可以通过 teleport 来进行挂载

```html
<teleport to="#title">
  <h2>xyj</h2>
</teleport>
```

![](https://gitee.com/itsandy/picgo-img/raw/master/vue/composition/teleport.png)

当然，teleport 也可以和组件结合一起来使用：

我们可以在 teleport 中使用组件，并且也可以给他传入一些数据；

如果我们将多个 teleport 应用到同一个目标上（to 的值相同），那么这些目标会进行合并

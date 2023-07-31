# 数组更新检测

Vue 将被侦听的数组的变更方法进行了包裹，所以它们也将会触发视图更新。这些被包裹过的方法包括：

详情：https://cn.vuejs.org/v2/guide/list.html#%E6%95%B0%E7%BB%84%E6%9B%B4%E6%96%B0%E6%A3%80%E6%B5%8B

```vue
<template>
  <div>
    <input type="text" v-model="message" />
    <button @click="addItem">添加</button>
    <ul>
      <li v-for="item in names" :key="item">{{ item }}</li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from 'vue';
const message = ref('');
const names = ref(['tao', 'xyj', 'zm', 'ymy']);
const addItem = () => {
  names.value.push(message.value);
  message.value = '';
};
</script>
```

## 一、v-for 中的 key 是什么作用？

在使用 v-for 进行列表渲染时，我们通常会给元素或者组件绑定一个 key 属性。

这个 key 属性有什么作用呢？我们先来看一下官方的解释： https://cn.vuejs.org/v2/api/#key

- key 属性主要用在 Vue 的虚拟 DOM 算法，在新旧 nodes 对比时辨识 VNodes；
- 如果不使用 key，Vue 会使用一种最大限度减少动态元素并且尽可能的尝试就地修改/复用相同类型元素的算法；
- 而使用 key 时，它会基于 key 的变化重新排列元素顺序，并且会移除/销毁 key 不存在的元素；

官方的解释对于初学者来说并不好理解，比如下面的问题：

- 什么是新旧 nodes，什么是 VNode？
- 没有 key 的时候，如何尝试修改和复用的？
- 有 key 的时候，如何基于 key 重新排列的？

## 二、认识 VNode

我们先来解释一下 VNode 的概念：

- 因为目前我们还没有比较完整的学习组件的概念，所以目前我们先理解 HTML 元素创建出来的 VNode；
- VNode 的全称是 Virtual Node，也就是虚拟节点；
- 事实上，无论是组件还是元素，它们最终在 Vue 中表示出来的都是一个个 VNode；
- VNode 的本质是一个 JavaScript 的对象；

![image.png](https://img11.360buyimg.com/ddimg/jfs/t1/61450/24/17363/10187/613f10a4Eee15d99d/4900c6cc39736c51.png)

## 三、虚拟 DOM

如果我们不只是一个简单的 div，而是有一大堆的元素，那么它们应该会形成一个 VNode Tree：

![image.png](https://img14.360buyimg.com/ddimg/jfs/t1/73668/17/16556/314426/613f117cE17093613/e31718d3df181780.png)

## 四、插入 F 的案例

我们先来看一个案例：这个案例是当我点击按钮时会在中间插入一个 f；

```vue
<template>
  <div>
    <button @click="addItem">添加</button>
    <ul>
      <li v-for="item in names" :key="item">{{ item }}</li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from "vue";
const names = ref(["a", "b", "c", "d"]);
const addItem = () => {
  names.value.splice(2, 0, "f");
};
</script>
```

我们可以确定的是，这次更新对于 ul 和 button 是不需要进行更新，需
要更新的是我们 li 的列表：

- 在 Vue 中，对于相同父元素的子元素节点并不会重新渲染整个列
  表；
- 因为对于列表中 a、b、c、d 它们都是没有变化的；
- 在操作真实 DOM 的时候，我们只需要在中间插入一个 f 的 li 即可；

那么 Vue 中对于列表的更新究竟是如何操作的呢？

- Vue 事实上会对于有 key 和没有 key 会调用两个不同的方法；
- 有 key，那么就使用 patchKeyedChildren 方法；
- 没有 key，那么就使用 patchUnkeyedChildren 方法；

## 五、Vue 源码对于 key 的判断

![image.png](https://img13.360buyimg.com/ddimg/jfs/t1/85283/37/20422/865786/613f2d45Eb1bd439a/c4f997c005fd2d5f.png)

## 六、没有 key 的操作

![image.png](https://img14.360buyimg.com/ddimg/jfs/t1/6884/7/13023/789360/613f2d5fEebcd1a2c/ccdc1019c392390c.png)

## 七、没有 key 的过程如下

我们会发现上面的 diff 算法效率并不高：

- c 和 d 来说它们事实上并不需要有任何的改动；
- 但是因为我们的 c 被 f 所使用了，所有后续所有的内容都要一次进行改动，并且最后进行新增；

![image.png](https://img10.360buyimg.com/ddimg/jfs/t1/61150/30/17519/315691/613f2d8cE20a97eb0/bf1275cbd2827023.png)

## 八、有 key 执行操作

![image.png](https://img14.360buyimg.com/ddimg/jfs/t1/62508/13/17277/868810/613f2dabE6dec1156/9020c819dd39301d.png)

## 九、有 key 的 diff 算法

第一步的操作是从头开始进行遍历、比较：

- a 和 b 是一致的会继续进行比较；
- c 和 f 因为 key 不一致，所以就会 break 跳出循环；

![image.png](https://img14.360buyimg.com/ddimg/jfs/t1/59644/19/17291/169613/613f2ddaE64c52033/a60c496e82f6ed35.png)

第二步的操作是从尾部开始进行遍历、比较：

![image.png](https://img12.360buyimg.com/ddimg/jfs/t1/207068/22/541/180470/613f2deaE5baa6daf/c643187e911bab54.png)

第三步是如果旧节点遍历完毕，但是依然有新的节点，那么就新增节点：

![image.png](https://img13.360buyimg.com/ddimg/jfs/t1/76221/32/17664/227218/613f2dfbE97bb699b/5cf98c9bff3c5582.png)

第四步是如果新的节点遍历完毕，但是依然有旧的节点，那么就移除旧节点：

![image.png](https://img13.360buyimg.com/ddimg/jfs/t1/207166/8/496/176860/613f2e0bE85f524ee/b19a27c17af5d845.png)

第五步是最特色的情况，中间还有很多未知的或者乱序的节点：

![image.png](https://img11.360buyimg.com/ddimg/jfs/t1/85264/24/16667/363030/613f2e21Ef78e0c50/d2816e7def047f47.png)

所以我们可以发现，Vue 在进行 diff 算法的时候，会尽量利用我们的 key 来进行优化操作：

- 在没有 key 的时候我们的效率是非常低效的；
- 在进行插入或者重置顺序的时候，保持相同的 key 可以让 diff 算法更加的高效；

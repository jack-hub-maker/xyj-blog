## JavaScript

详情可见：[https://github.com/airbnb/javascript](https://github.com/airbnb/javascript)

还有一个阮一峰老师的 JavaScript 编程风格[https://es6.ruanyifeng.com/#docs/style](https://es6.ruanyifeng.com/#docs/style)

## Vue

详情可见:[https://v3.cn.vuejs.org/style-guide/](https://v3.cn.vuejs.org/style-guide/)

这里我列出我认同的一些规范

### Prop 定义

**Prop 定义应尽量详细**

在你提交的代码中，prop 的定义应该尽量详细，至少需要指定其类型

!> 细致的 Props 定义有两个好处：可读性强，如果向组件提供了格式不正确的 props,Vue 会给你很好的警示，方便你找到 bug 的来源

> [!TIP]
>
> ```js
> props: {
>   name: {
>     type: String,
>   },
> },
> ```

### 为 v-for 设置 key 值

**总是用 key 配合 v-for**

!> 优化 diff 算法，提升性能，详情可见[占位]()

> [!TIP]
>
> ```js
> <ul>
>  <li
>    v-for="todo in todos"
>    :key="todo.id"
>  >
>    {{ todo.text }}
>  </li>
> </ul>
> ```

### 避免 v-if 和 v-for 一起使用

**永远不要把 v-if 和 v-for 同时用在同一个元素上。**

!> 当它们处于同一节点，v-if 的优先级比 v-for 更高，这意味着 v-if 将没有权限访问 v-for 里的变量：

> [!Danger]
>
> ```js
> <ul>
>   <li
>     v-for="user in users"
>     v-if="user.isActive"
>     :key="user.id"
>   >
>     {{ user.name }}
>   </li>
> </ul>
> ```
>
> 因为 v-if 的优先级高于 v-for，所以 v-if 执行的时候 user 是不存在的，则会报错

### 为组件样式设置作用域

**对于应用来说，顶层 App 组件和布局组件中的样式可以是全局的，但是其它所有组件都应该是有作用域的。**

!> 如果你和其他开发者一起开发一个大型工程，或有时引入三方 HTML/CSS，设置一致的作用域会确保你的样式只会运用在它们想要作用的组件上。

> [!TIP]
>
> ```css
> <style scoped>
> .button {
>  border: none;
>  border-radius: 2px;
> }
>
> .button-close {
>  background-color: red;
> }
> </style>
> ```

### 私有 property 名称

使用`$_` 前缀并附带 `property` 来定义私有属性

!> 一般定义私有属性是用为前缀,但是 vue 中使用前缀，有可能会覆盖没有前缀名的 property 对于`$`前缀来说,vue 中`$`属于特殊的实例 property，所以用它来用于私有属性也不合适 vue 推荐把两个前缀结合在一起，成为`$_`作为定义私有属性的约定,以确保不会和 vue 自身产生冲突

> [!TIP]
>
> ```js
>  methods: {
>      $_myBtnClick()
>    }
>  }
> ```

### 单文件组件文件的大小写

**单文件组件的文件名应该要么始终是单词大写开头 (PascalCase)，要么始终是横线连接 (kebab-case)。**

!> 单词大写开头对于代码编辑器的自动补全最为友好，因为这使得我们在 JS(X) 和模板中引用组件的方式尽可能的一致。然而，混用文件命名方式有的时候会导致大小写不敏感 文件系统的问题，这也是横线连接命名同样完全可取的原因。

> [!TIP]
>
> ```sh
> components/
> |- MyComponent.vue
> ```

### 单组件名称

**只应该拥有单个活跃实例的组件应该以 `The` 前缀命名，以示其唯一性。**

> [!TIP]
>
> ```sh
> components/
> |- TheHeading.vue
> |- TheSidebar.vue
> ```

### 紧密耦合的组件名称

**和父组件紧密耦合的子组件应该以父组件名作为前缀命名。**

!> 如果一个组件只在某个父组件的场景下有意义，这层关系应该体现在其名字上。因为编辑器通常会按字母顺序组织文件，所以这样做可以把相关联的文件排在一起。

> [!TIP]
>
> ```sh
> components/
> |- TodoList.vue
> |- TodoListItem.vue
> |- TodoListItemButton.vue
> ```

### 组件名称中的单词顺序

!> 组件名称应该以高阶的 (通常是一般化描述的) 单词开头，以描述性的修饰词结尾。

> [!TIP]
>
> ````sh
> components/
> |- SearchButtonClear.vue
> |- SearchButtonRun.vue
> |- SearchInputQuery.vue
> |- SearchInputExcludeGlob.vue
> |- SettingsCheckboxTerms.vue
> |- SettingsCheckboxLaunchOnStartup.vue
> ```
> ````

### 自闭合组件

**在单文件组件、字符串模板和 JSX 中没有内容的组件应该是自闭合的——但在 DOM 模板里永远不要这样做。**

> [!TIP]
>
> ```html
> <my-button />
> ```

### 模板中的组件名称大小写

**对于绝大多数项目来说，在单文件组件和字符串模板中组件名称应该总是 PascalCase 的——但是在 DOM 模板中总是 kebab-case 的。**

> [!TIP]
>
> ````html
> <!-- 在单文件组件和字符串模板中 -->
> <MyComponent /> ><!-- 在 DOM 模板中            -->
> <my-component></my-component> ><!-- 在所有地方 -->
> <my-component></my-component> >```
> ````

### JS/JSX 中使用的组件名称

**JS/JSX 中的组件名应该始终是 PascalCase 的**

> [!TIP]
>
> ```js
> import MyComponent from "./MyComponent.vue";
> ```

### 完整单词的组件名称

**组件名称应该倾向于完整单词而不是缩写。**

!>编辑器中的自动补全已经让书写长命名的代价非常之低了，而其带来的明确性却是非常宝贵的。不常用的缩写尤其应该避免。

> [!TIP]
>
> ```sh
> components/
> |- StudentDashboardSettings.vue
> |- UserProfileOptions.vue
> ```

### Prop 名称

**在声明 prop 的时候，其命名应该始终使用 camelCase，而在模板和 JSX 中应该始终使用 kebab-case。**

!>我们单纯的遵循每个语言的约定。在 JavaScript 中更自然的是 camelCase。而在 HTML 中则是 kebab-case。

> [!TIP]
>
> ```js
> props: {
>   greetingText: String;
> }
> ```
>
> ```html
> <WelcomeMessage greeting-text="hi" />
> ```

### 多个 attribute 的元素

**多个 attribute 的元素应该分多行撰写，每个 attribute 一行。**

!>在 JavaScript 中，用多行分隔对象的多个 property 是很常见的最佳实践，因为这样更易读。模板和 JSX 值得我们做相同的考虑。

### 模板中的简单表达式

**组件模板应该只包含简单的表达式，复杂的表达式则应该重构为计算属性或方法。**

!>复杂表达式会让你的模板变得不那么声明式。我们应该尽量描述应该出现的是什么，而非如何计算那个值。而且计算属性和方法使得代码可以重用。

> [!TIP]
>
> ```html
> <!-- 在模板中 -->
> {{ normalizedFullName }}
> ```
>
> ```js
> // 复杂表达式已经移入一个计算属性
> computed: {
>  normalizedFullName() {
>    return this.fullName.split(' ')
>      .map(word => word[0].toUpperCase() + word.slice(1))
>      .join(' ')
>  }
> }
> ```

### 简单的计算属性

**应该把复杂计算属性分割为尽可能多的更简单的 property。**

!>易于调试，易于阅读，更好的拥抱变化

> [!Danger]
>
> ```js
> computed: {
>  price() {
>    const basePrice = this.manufactureCost / (1 - this.profitMargin)
>    return (
>      basePrice -
>      basePrice * (this.discountPercent || 0)
>    )
>  }
> }
> ```

> [!TIP]
>
> ```js
> omputed: {
>   basePrice() {
>     return this.manufactureCost / (1 - this.profitMargin)
>   },
>
>   discount() {
>     return this.basePrice \* (this.discountPercent || 0)
>   },
>
>   finalPrice() {
>     return this.basePrice - this.discount
>   }
> }
> ```

### 指令缩写

**指令缩写 (用 : 表示 v-bind:，@ 表示 v-on: 和用 # 表示 v-slot) 应该要么都用要么都不用。**

### 组件/实例选项中的空行

**你可能想在多个 property 之间增加一个空行，特别是在这些选项一屏放不下，需要滚动才能都看到的时候。**

!>当你的组件开始觉得密集或难以阅读时，在多个 property 之间添加空行可以让其变得容易。在一些诸如 Vim 的编辑器里，这样格式化后的选项还能通过键盘被快速导航。

> [!TIP]
>
> ```js
> // 没有空行在组件易于阅读和导航时也没问题。
> props: {
>  value: {
>    type: String,
>    required: true
>  },
>
>  focused: {
>    type: Boolean,
>    default: false
>  },
>
>  label: String,
>  icon: String
> },
>
> computed: {
>  formattedValue() {
>    // ...
>  },
>
>  inputClasses() {
>    // ...
>  }
> }
> ```

### scoped 中的元素选择器

**元素选择器应该避免在 scoped 中出现。**

!>在 scoped 样式中，类选择器比元素选择器更好，因为大量使用元素选择器是很慢的。

# 企业开发编程规范

## 一、为什么需要编程规范？

**工欲善其事，必先利其器**

对于一些大型的企业级项目而言，通常情况下我们都是需要一个团队来进行开发的。而又因为团队人员对技术理解上的参差不齐，所以就会导致出现一种情况，那就是《**一个项目无法具备统一的编程规范，导致项目的代码像多个不同材质的补丁拼接起来一样**》

设想一下，下面的这段代码有一个团队进行开发，因为没有具备统一的代码标准，所以生成了下面的代码：


这段代码可以正常运行没有问题，但是整体的代码结构却非常的难看。

这样的项目虽然可以正常运行，但是如果把它放到大厂的项目中，确实 **不及格** 的，它会被认为是 **不可维护、不可扩展的代码内容**

那么所谓的大厂标准的代码结构应该是什么样子的呢？

我们把上面的代码进行一下修正，做一个对比：


修改之后的代码具备了统一的规范之后，是不是看起来就舒服多了！

并且以上所列举出来的只是《编程规范》中的一小部分内容！

那么有些朋友可能就会说了，你列举出来这些编程规范有什么用啊！

哪怕你写上一部书，我们一个团队这么多人，总不能指望所有人都看一遍，并且严格的遵守你所说的规范吧！

说的没错！指望人主动的遵守这些规范不太现实

那怎么办呢？

那么我们可不可以另辟蹊径，让程序自动处理规范化的内容呢？

答案是：可以的！

这些也是我们本章节所需要讲解的重点内容！

本章节中我们会为大家讲解，如何自动化的对代码进行规范，其中主要包括：

1. 编码规范
2. git 规范

两大类

那么明确好了我们的范围之后，接下来就让我们创建一个项目，开始我们的代码规范之旅吧！

## 二、编码风格

在企业多人开发中，每个人都有自己的编码风格，我喜欢缩进两行，你喜欢缩进四行，我喜欢单引号，你喜欢多引号

这样的话项目就存在各种各样的代码风格，我们需要一个东西来统一我们的编码风格

[editorconfig](https://editorconfig.org/) 有助于为不同 IDE 编辑器上处理同一项目的多个开发人员维护一致的编码风格。

在根目录下创建.editorconfig 文件,在文件中编写对应的约束

如果你使用的是 vscode 的话，这里我推荐一个插件`EditorConfig for VS Code`


## 三、编码规范

### 3.1 代码检测工具 ESLint

在我们去创建项目的时候，脚手架工具已经帮助我们安装了 [ESLint](https://github.com/eslint/eslint) 代码检测工具。

对于 `ESLint` 的大名，朋友们或多或少的应该都听说过，只不过有些朋友可能了解的多一些，有些朋友了解的少一些。

那么本小节我们就先来聊一下，这个赫赫有名的代码检测工具 `ESLint`

首先 `ESLint` 是 `2013年6月` 创建的一个开源项目，它的目标非常简单，只有一个，那就是 **提供一个插件化的 `javascript` 代码检测工具** ，说白了就是做 **代码格式检测使用的**

在咱们当前的项目中，包含一个 `.eslintrc.js` 文件，这个文件就是 `eslint` 的配置文件。

随着大家对代码格式的规范性越来越重视，`eslint` 也逐渐被更多的人所接收，同时也有很多大厂在原有的 `eslint` 规则基础之上进行了一些延伸。

我们在创建项目时，就进行过这样的选择：


我们先选择 **标准的 ESLint 规则** ，那么接下来我们就在该规则之下，看一看 `ESLint` 它的一些配置都有什么？

打开项目中的 `.eslintrc.js` 文件

```js
// ESLint 配置文件遵循 commonJS 的导出规则，所导出的对象就是 ESLint 的配置对象
// 详细文档: https://eslint.org/docs/user-guide/configuring/
module.exports = {
  // 表示当前目录即为根目录，ESLint 规则将被限制到该目录下
  root: true,
  // env 表示启用 ESLint 检测的环境
  env: {
    // 在 node 环境下启动 ESLint 检测
    node: true,
  },
  // ESLint 中基础配置需要继承的配置
  extends: ["plugin:vue/essential", "@vue/standard"],
  // 解析器
  parserOptions: {
    parser: "babel-eslint",
  },
  // 需要修改的启用规则及其各自的错误级别
  /**
   * 错误级别分为三种：
   * "off" 或 0 - 关闭规则
   * "warn" 或 1 - 开启规则，使用警告级别的错误：warn (不会导致程序退出)
   * "error" 或 2 - 开启规则，使用错误级别的错误：error (当被触发的时候，程序会退出)
   * */
  rules: {
    "no-console": process.env.NODE_ENV === "production" ? "warn" : "off",
    "no-debugger": process.env.NODE_ENV === "production" ? "warn" : "off",
    indent: "off",
    "space-before-function-paren": "off",
  },
};
```

那么到这里咱们已经大致的了解了.eslintrc.js 文件，基于 ESLint 如果我们出现不符合规范的代码格式时，那么就会得到一个对应的错误。

比如: 我们可以把 Home.vue 中的 name 属性值，由单引号改为双引号

此时，只要我们一保存代码，那么就会得到一个对应的错误


这个错误表示：

1. 此时我们触发了一个 《错误级别的错误》
2. 发该错误的位置是 在 Home.vue 的第 13 行 第九列 中
3. 错误描述为：字符串必须使用单引号
4. 错误规则为：quotes

那么想要解决这个错误，通常情况下我们有两种方式：

1.  按照 ESLint 的要求修改代码
2.  修改 ESLint 的验证规则

按照 ESLint 的要求修改代码：

在 Home.vue 的第 13 行中把双引号改为单引号

修改 ESLint 的验证规则：

在 .eslintrc.js 文件中，新增一条验证规则

```js
"quotes": "error" // 默认
"quotes": "warn" // 修改为警告
"quotes": "off" // 修改不校验
```

这样我们算是对 ESLint 做了一个简单的邂逅,Standard config 的 ESLint 配置，并且知道了如何解决 ESLint 报错的问题。

但是一个团队中，人员的水平高低不齐，大量的 ESLint 规则校验，会让很多的开发者头疼不已，从而大大影响了项目的开发进度。

试想一下，在你去完成项目代码的同时，还需要时时刻刻注意代码的格式问题，这将是一件多么痛苦的事情！

那么有没有什么办法，既可以保证 ESLint 规则校验，又可以解决严苛的格式规则导致的影响项目进度的问题呢？

这个时候我们就可以使用 Prettier ，让你的代码变得更漂亮！

### 3.2 代码格式化 Prettier

我们知道了 ESLint 可以让我们的代码格式变得更加规范，但是同样的它也会带来开发时编码复杂度上升的问题。

那么有没有办法既可以保证 ESLint 规则校验，又可以让开发者无需关注格式问题来进行顺畅的开发呢？

答案是：有的！

而解决这个问题的关键就是 [Prettier](https://github.com/prettier/prettier)

Prettier 是一款强大的代码格式化工具，支持 JavaScript、TypeScript、CSS、SCSS、Less、JSX、Angular、Vue、GraphQL、JSON、Markdown 等语言，基本上前端能用到的文件格式它都可以搞定，是当下最流行的**代码格式化工具**。

那么这些简单配置具体指的是什么呢？

我们提到《prettier 可以在保存代码时，让我们的代码直接符合 ESLint 标准》但是想要实现这样的功能需要进行一些配置。

那么这一小节，我们就来去完成这个功能：

1.  在 VSCode 中安装 prettier 插件，这个插件可以帮助我们在配置 prettier 的时候获得提示


2.  在项目中新建 `.prettierrc` 文件，该文件为 perttier 默认配置文件
3.  在该文件中写入如下配置：

```json
{
  // 不尾随分号
  "semi": false,
  // 使用单引号
  "singleQuote": true,
  // 多行逗号分割的语法中，最后一行不加逗号
  "trailingComma": "none"
}
```

4.  打开 VSCode 《设置面板》


5.  在设置中，搜索 `save` ，勾选 `Format On Save`


6.  至此，你即可在 **VSCode 保存时，自动格式化代码！**

最后再推荐你配置格式化选项

右键你想要配置格式化项目的文件


然后选择 Prettier


**但是！** 你只做到这样还不够！

- VSCode 而言，默认一个 tab 等于 4 个空格，而 ESLint 希望一个 tab 为两个空格
- 如果大家的 VSCode 安装了多个代码格式化工具
- ESLint 和 prettier 之间的冲突问题

比如:我们尝试在 Home.vue 中写入一个 created 方法，写入完成之后，打开我们的控制台我们会发现，此时代码抛出了一个 ESLint 的错误


这个错误的意思是说：**created 这个方法名和后面的小括号之间，应该有一个空格！**

但是当我们加入了这个空格之后，只要一保存代码，就会发现 prettier 会自动帮助我们去除掉这个空格。

这个时候可以安装一个插件

```sh
pnpm i eslint-plugin-prettier eslint-config-prettier -D
```

如果你在使用 cli 开发项目,选择了 prettier+eslint 的话,那么这两个插件会自动安装

修改完配置之后我们需要重启项目

至此我们整个的 perttier 和 ESLint 的配合使用就算是全部完成了。

在之后我们写代码的过程中，只需要保存代码，那么 perttier 就会帮助我们自动格式化代码，使其符合 ESLint 的校验规则。而无需我们手动进行更改了。

## 四、Git 提交规范

在前面我们通过 `Prettier + ESLint` 解决了代码格式的问题，但是我们之前也说过 **编程规范** 指的可不仅仅只是 **代码格式规范** 。

除了 **代码格式规范** 之外，还有另外一个很重要的规范就是 **git 提交规范！**

在现在的项目开发中，通常情况下，我们都会通过 git 来管理项目。只要通过 git 来管理项目，那么就必然会遇到使用 git 提交代码的场景

当我们执行 `git commit -m "描述信息"` 的时候，我们知道此时必须添加一个描述信息。但是中华文化博大精深，不同的人去填写描述信息的时候，都会根据自己的理解来进行描述。

而很多人的描述 “天马行空” ，这样就会导致别人在看你的提交记录时，看不懂你说的什么意思？不知道你当前的这次提交到底做了什么事情？会不会存在潜在的风险？

比如说，我们来看这几条提交记录：


你能够想象得到它们经历了什么吗？

所以 git 提交规范 势在必行。

对于 git 提交规范 来说，不同的团队可能会有不同的标准，那么咱们今天就以目前使用较多的 [Angular 团队规范](https://github.com/angular/angular.js/blob/master/DEVELOPERS.md#-git-commit-guidelines) 延伸出的 [Conventional Commits specification（约定式提交）](https://www.conventionalcommits.org/zh-hans/v1.0.0/) 为例，来为大家详解 git 提交规范

约定式提交规范要求如下：

```
<type>[optional scope]: <description>

​[optional body]​[optional footer(s)]​

--------- 翻译 -------------  

<类型>[可选 范围]: <描述>

​[可选 正文]

​[可选 脚注]
```

其中 `<type>` 类型，必须是一个可选的值，比如：

1. 新功能：`feat`
2. 修复：`fix`
3. 文档变更：`docs`
4. ....

也就是说，如果要按照 **约定式提交规范** 来去做的化，那么你的一次提交描述应该式这个样子的：

我想大家看到这样的一个提交描述之后，心里的感觉应该和我一样是崩溃的！要是每次都这么写，写到猴年马月了！

如果你有这样的困惑，那么 ”恭喜你“ ，接下来我们将一起解决这个问题！

### 4.1 代码提交规范

我们知道如果严格安装 约定式提交规范， 来手动进行代码提交的话，那么是一件非常痛苦的事情，但是 git 提交规范的处理 又势在必行，那么怎么办呢？

你遇到的问题，也是其他人所遇到的！

经过了很多人的冥思苦想，就出现了一种叫做 git 提交规范化工具 的东西，而我们要学习的 commitizen 就是其中的佼佼者！

**commitizen** 仓库名为 [cz-cli](https://github.com/commitizen/cz-cli) ，它提供了一个 `git cz` 的指令用于代替 `git commit`，简单一句话介绍它：

?> 当你使用 commitizen 进行代码提交（git commit）时，commitizen 会提交你在提交时填写所有必需的提交字段！

这句话怎么解释呢？不用着急，下面我们就来安装并且使用一下 `commitizen` ，使用完成之后你自然就明白了这句话的意思！

1.  全局安装`Commitizen`

```sh
pnpm i -g commitizen
```

你可以使用 npm 或者 yarn 安装,这里我使用[pnpm](https://github.com/pnpm/pnpm)安装

!>注意使用 pnpm 安装都需要设置 npm 的镜像一定要为 taobao 才可以(如下都是),否则会报这样的错误


2.  安装并配置 [`cz-customizable`](https://github.com/leoforfree/cz-customizable) 插件
    - 使用 npm 下载 cz-customizable

```sh
pnpm i cz-customizable -D
```

添加以下配置到 `package.json ` 中

```json
"config": {
  "commitizen": {
    "path": "node_modules/cz-customizable"
  }
}
```

3.  项目根目录下创建 `.cz-config.js` 自定义提示文件

```js
module.exports = {
  // 可选类型
  types: [
    { value: "feat", name: "feat:     新功能" },
    { value: "fix", name: "fix:      修复" },
    { value: "docs", name: "docs:     文档变更" },
    { value: "style", name: "style:    代码格式(不影响代码运行的变动)" },
    {
      value: "refactor",
      name: "refactor: 重构(既不是增加feature，也不是修复bug)",
    },
    { value: "perf", name: "perf:     性能优化" },
    { value: "test", name: "test:     增加测试" },
    { value: "chore", name: "chore:    构建过程或辅助工具的变动" },
    { value: "revert", name: "revert:   回退" },
    { value: "build", name: "build:    打包" },
  ],
  // 消息步骤
  messages: {
    type: "请选择提交类型:",
    customScope: "请输入修改范围(可选):",
    subject: "请简要描述提交(必填):",
    body: "请输入详细描述(可选):",
    footer: "请输入要关闭的issue(可选):",
    confirmCommit: "确认使用以上信息提交？(y/n/e/h)",
  },
  // 跳过问题
  skipQuestions: ["body", "footer"],
  // subject文字长度默认是72
  subjectLimit: 72,
};
```

4.  使用 `git cz` 代替 `git commit`

使用 git cz 代替 git commit，即可看到提示内容

那么到这里我们就已经可以使用 git cz 来代替了 git commit 实现了规范化的提交诉求了，但是当前依然存在着一个问题，那就是我们必须要通过 git cz 指令才可以完成规范化提交！

如果觉得配置很麻烦的话，这里我推荐使用另外一个插件 [cz-conventional-changelog](https://github.com/commitizen/cz-conventional-changelog)

安装 cz-conventional-changelog，并且初始化 cz-conventional-changelog

```sh
npx commitizen init cz-conventional-changelog --save-dev --save-exact
```

安装完毕之后在 package.json 中进行配置：

```json
"config": {
  "commitizen": {
    "path": "node_modules/cz-conventional-changelog"
  }
}
```

这个时候我们提交代码需要使用 npx cz：

- 第一步是选择 type，本次更新的类型

| Type     | 作用                                                                                   |
| -------- | -------------------------------------------------------------------------------------- |
| feat     | 新增特性 (feature)                                                                     |
| fix      | 修复 Bug(bug fix)                                                                      |
| docs     | 修改文档 (documentation)                                                               |
| style    | 代码格式修改(white-space, formatting, missing semi colons, etc)                        |
| refactor | 代码重构(refactor)                                                                     |
| perf     | 改善性能(A code change that improves performance)                                      |
| test     | 测试(when adding missing tests)                                                        |
| build    | 变更项目构建或外部依赖（例如 scopes: webpack、gulp、npm 等）                           |
| ci       | 更改持续集成软件的配置文件和 package 中的 scripts 命令，例如 scopes: Travis, Circle 等 |
| chore    | 变更构建流程或辅助工具(比如更改测试环境)                                               |
| revert   | 代码回退                                                                               |

- 第二步选择本次修改的范围（作用域）

![image-20210723150147510](https://tva1.sinaimg.cn/large/008i3skNgy1gsqw8ca15oj30r600wmx4.jpg)

- 第三步选择提交的信息

![image-20210723150204780](https://tva1.sinaimg.cn/large/008i3skNgy1gsqw8mq3zlj60ni01hmx402.jpg)

- 第四步提交详细的描述信息

![image-20210723150223287](https://tva1.sinaimg.cn/large/008i3skNgy1gsqw8y05bjj30kt01fjrb.jpg)

- 第五步是否是一次重大的更改

![image-20210723150322122](https://tva1.sinaimg.cn/large/008i3skNgy1gsqw9z5vbij30bm00q744.jpg)

- 第六步是否影响某个 open issue

![image-20210723150407822](https://tva1.sinaimg.cn/large/008i3skNgy1gsqwar8xp1j30fq00ya9x.jpg)

我们也可以在 scripts 中构建一个命令来执行 cz：

![image-20210723150526211](https://tva1.sinaimg.cn/large/008i3skNgy1gsqwc4gtkxj30e207174t.jpg)

那么如果有马虎的同事，它们忘记了使用 git cz 指令，直接就提交了怎么办呢？

那么有没有方式来限制这种错误的出现呢？

答案是有的！

### 4.2 代码提交验证

先来明确一下我们最终要实现的效果：

我们希望：当《提交描述信息》不符合 约定式提交规范 的时候，阻止当前的提交，并抛出对应的错误提示

而要实现这个目的，我们就需要先来了解一个概念，叫做 `Git hooks（git 钩子 || git 回调方法）`

也就是：git 在执行某个事件之前或之后进行一些其他额外的操作

而我们所期望的 阻止不合规的提交消息，那么就需要使用到 `hooks` 的钩子函数。

PS：详细的 hooks 介绍 可点击[这里](https://git-scm.com/docs/githooks)查看

整体的 `hooks` 非常多，当时我们其中用的比较多的其实只有两个：

| Git Hook       | 调用时机                                                                                                                                          | 说明                               |
| :------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------- |
| **pre-commit** | git commit 执行前<br />它不接受任何参数，并且在获取提交日志消息并进行提交之前被调用。脚本 git commit 以非零状态退出会导致命令在创建提交之前中止。 | 可以用`git commit --no-verify`绕过 |
| **commit-msg** | git commit 执行前<br />可用于将消息规范化为某种项目标准格式。<br />还可用于在检查消息文件后拒绝提交。                                             | 可以用`git commit --no-verify`绕过 |

而我们接下来要做的关键，就在这两个钩子上面。

我们了解了 git hooks 的概念，那么接下来我们就使用 git hooks 来去校验我们的提交信息。

要完成这么个目标，那么我们需要使用两个工具：

1. [commitlint](https://github.com/conventional-changelog/commitlint)：用于检查提交信息

2. [husky](https://github.com/typicode/husky)：是 git hooks 工具

那么下面我们分别来去安装一下这两个工具：

**commitlint**

1.  安装依赖：

```sh
pnpm i @commitlint/config-conventional @commitlint/cli -D
```

2.  创建 `commitlint.config.js` 文件

```sh
echo "module.exports = {extends: ['@commitlint/config-conventional']}" > commitlint.config.js
```

3.  打开 `commitlint.config.js`,增加配置项[config-conventional 默认配置点击可查看](https://github.com/conventional-changelog/commitlint/blob/master/@commitlint/config-conventional/index.js)

```js
module.exports = {
  // 继承的规则
  extends: ["@commitlint/config-conventional"],
  // 定义规则类型
  rules: {
    // type 类型定义，表示 git 提交的 type 必须在以下类型范围内
    "type-enum": [
      2,
      "always",
      [
        "feat", // 新功能 feature
        "fix", // 修复 bug
        "docs", // 文档注释
        "style", // 代码格式(不影响代码运行的变动)
        "refactor", // 重构(既不增加新功能，也不是修复bug)
        "perf", // 性能优化
        "test", // 增加测试
        "chore", // 构建过程或辅助工具的变动
        "revert", // 回退
        "build", // 打包
      ],
    ],
    // subject 大小写不做校验
    "subject-case": [0],
  },
};
```

!> 注意：确保保存为 `UTF-8` 的编码格式，否则可能会出现以下错误：


接下来我们来安装 husky

1.  安装依赖：

```sh
pnpm i husky -D
```

2.  启动 hooks， 生成`.husky` 文件夹

```sh
npx husky install
```


3.  在 package.json 中生成 `prepare` 指令

```sh
npm set-script prepare "husky install"
```

4.  执行 prepare 指令

```sh
pnpm run prepare
```

5.  执行成功，提示


6.  添加 commitlint 的 hook 到 husky 中，并指令在 commit-msg 的 hooks 下执行 `npx --no-install commitlint --edit "$1"` 指令

```sh
npx husky add .husky/commit-msg 'npx --no-install commitlint --edit "$1"'
```

7.  此时的 `.husky` 的文件结构


至此， 不符合规范的 commit 将不再可提交：

那么至此，我们就已经可以处理好了 强制规范化的提交要求，到现在 不符合规范的提交信息，将不可在被提交

那么到这里我们的 规范化目标 就完成了吗？

当然没有！

现在我们还缺少一个 规范化的处理 ，那就是 代码格式提交规范处理！

有朋友看到这里可能说，咦！ 这个怎么看着这么眼熟啊？这个事情我们之前不是做过了吗？还需要在处理什么？

在 ESLint 与 Prettier 配合解决代码格式问题 的章节中，我们讲解了如何处理 本地！代码格式问题

但是这样的一个格式处理问题，他只能够在本地进行处理，并且我们还需要 手动在 VSCode 中配置自动保存 才可以。那么这样就会存在一个问题，要是有人忘记配置这个东西了怎么办呢？他把代码写的乱七八糟的直接就提交了怎么办呢？

所以我们就需要有一种方式来规避这种风险。

那么想要完成这么一个操作就需要使用 husky 配合 eslint 才可以实现。

我们期望通过 husky 监测 pre-commit 钩子，在该钩子下执行 `npx eslint --ext .js,.vue src` 指令来去进行相关检测：

1.  执行 添加 commit 时的 hook （npx eslint --ext .js,.vue src 会在执行到该 hook 时运行）

```sh
npx husky add .husky/pre-commit "npx eslint --ext .js,.vue src"
```

2.  该操作会生成对应文件 `pre-commit`：


3.  修改一处代码，使其不符合 ESLint 校验规则

4.  执行 提交操作 会发现，抛出一系列的错误，代码无法提交

那么到这里位置，我们已经通过 `pre-commit` 检测到了代码的提交规范问题。

那么到这里就万事大吉了吗？

在这个世界上从来不缺的就是懒人，错误的代码格式可能会抛出很多的 `ESLint` 错误，让人看得头皮发麻。严重影响程序猿的幸福指数。

那么有没有办法，让程序猿在 0 配置的前提下，哪怕代码格式再乱，也可以 **”自动“** 帮助他修复对应的问题，并且完成提交呢？

你别说，还真有！

但是这样会存在两个问题：

1. 我们只修改了个别的文件，没有必要检测所有的文件代码格式
2. 它只能给我们提示出对应的错误，我们还需要手动的进行代码修改

那么这一小节，我们就需要处理这两个问题

那么想要处理这两个问题，就需要使用另外一个插件 [lint-staged](https://github.com/okonet/lint-staged) ！

lint-staged 可以让你当前的代码检查 **只检查本次修改更新的代码，并在出现错误的时候，自动修复并且推送**

lint-staged 无需单独安装，我们生成项目时，`vue-cli` 已经帮助我们安装过了，所以我们直接使用就可以了

1.  修改 package.json 配置

```json
"lint-staged": {
  "src/**/*.{js,vue}": [
    "eslint --fix",
    "git add"
  ]
}
```

2.  如上配置，每次它只会在你本地 commit 之前，校验你提交的内容是否符合你本地配置的 eslint 规则，校验会出现两种结果：

    - 如果符合规则：则会提交成功。
    - 如果不符合规则：它会自动执行 `eslint --fix` 尝试帮你自动修复，如果修复成功则会帮你把修复好的代码提交，如果失败，则会提示你错误，让你修好这个错误之后才能允许你提交代码。

3.  修改 `.husky/pre-commit` 文件

```
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

npx lint-staged
```

4.  再次执行提交代码

5.  发现 **暂存区中** 不符合 ESlint 的内容，被自动修复

?>那么完成这些规范操作之后，请放心大胆将这些在项目中使用并安利其它同事一起使用 🤞💗

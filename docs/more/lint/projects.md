## 集成 editorconfig 配置

EditorConfig 有助于为不同 IDE 编辑器上处理同一项目的多个开发人员维护一致的**编码风格**。

📖 官网:[http://editorconfig.org](http://editorconfig.org)

根据官网的详细介绍,进行自定义配置

如果觉得有些困难的话,这里推荐一种方法

比如你想要开发 vue 的项目,可以去 vue 的 GitHub 地址查看相关的配置

![1.png](https://img11.360buyimg.com/ddimg/jfs/t1/178585/34/18632/89036/611223faEf0eee40c/38d4b13d1df0775e.png)

![2.png](https://img10.360buyimg.com/ddimg/jfs/t1/177124/40/18600/22706/611223fbE53738c26/e13db96de990fc4d.png)

在根目录下创建.editorconfig 文件,在文件中编写对应的约束

![3.png](https://img11.360buyimg.com/ddimg/jfs/t1/200625/18/1237/25975/611223f8E9c09aa5c/bca79b568a04cf25.png)

另外 vscode 还需要安装一个插件:EditorConfig for VS Code

## 使用 prettier 工具

Prettier 是一款强大的代码格式化工具，支持 JavaScript、TypeScript、CSS、SCSS、Less、JSX、Angular、Vue、GraphQL、JSON、Markdown 等语言，基本上前端能用到的文件格式它都可以搞定，是当下最流行的**代码格式化工具**。

1.安装 prettier

```shell
npm install prettier -D
```

2.根目录配置.prettierrc 文件：

📖 官网:[https://prettier.io/](https://prettier.io/)

另外 vscode 还需要安装一个插件:Prettier - Code formatter

![4.png](https://img11.360buyimg.com/ddimg/jfs/t1/202811/26/652/36107/611223f8E8b336830/6818b18d3ad816ff.png)

## 使用 ESLint 检测

如果我们开发 vue 的项目,使用了 cli 进行开发,在进行配置的时候选择了 eslint,那么 vue 会默认帮助我们配置需要的环境
![5.png](https://img11.360buyimg.com/ddimg/jfs/t1/187253/26/17387/22353/611223f8E5358c37a/b9a39d673ab4b68e.png)

如果没有的话可以通过在.eslintrc.js 文件中进行配置

如果有些文件不想被 eslint 约束的话可以在.eslintignore 中进行配置(跟.gitignore 相似)

在开发中一般 Prettier 都会和 eslint 的规则产生冲突

这个时候可以安装一个插件

```shell
npm i eslint-plugin-prettier eslint-config-prettier -D
```

如果你在使用 cli 开发项目,选择了 prettier+eslint 的话,那么这两个插件会自动安装

然后在.eslintrc.js 文件中进行相关配置

📖 官网:[https://eslint.org/](https://eslint.org/)

## git Husky 和 eslint

虽然我们已经要求项目使用 eslint 了，但是不能保证组员提交代码之前都将 eslint 中的问题解决掉了：

- 也就是我们希望保证代码仓库中的代码都是符合 eslint 规范的；

- 那么我们需要在组员执行 `git commit` 命令的时候对其进行校验，如果不符合 eslint 规范，那么自动通过规范进行修复；

那么如何做到这一点呢？可以通过 Husky 工具：

- husky 是一个 git hook 工具，可以帮助我们触发 git 提交的各个阶段：pre-commit、commit-msg、pre-push

如何使用 husky 呢？

这里我们可以使用自动配置命令：

```shell
npx husky-init;npm install
```

这里命令会做三件事：

- 安装 husky 相关的依赖：

- 在项目目录下创建 `.husky` 文件夹

- 在 package.json 中添加一个脚本

接下来，我们需要去完成一个操作：在进行 commit 时，执行 lint 脚本：

![6.png](https://img11.360buyimg.com/ddimg/jfs/t1/188472/2/17681/23281/611223f8Ef00913dc/09634941c1a620a7.png)

这个时候我们执行 git commit 的时候会自动对代码进行 lint 校验。

![7.png](https://img11.360buyimg.com/ddimg/jfs/t1/204369/6/608/20908/611223f8E7129e06e/52a00d70cccc6ca0.png)

## git commit 规范

### 代码提交风格

通常我们的 git commit 会按照统一的风格来提交，这样可以快速定位每次提交的内容，方便之后对版本进行控制。

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsqw17gaqjj30to0cj3zp.jpg)

但是如果每次手动来编写这些是比较麻烦的事情，我们可以使用一个工具：Commitizen

- Commitizen 是一个帮助我们编写规范 commit message 的工具；

  1.安装 Commitizen

```shell
npm install commitizen -D
```

2.安装 cz-conventional-changelog，并且初始化 cz-conventional-changelog：

```shell
npx commitizen init cz-conventional-changelog --save-dev --save-exact
```

这个命令会帮助我们安装 cz-conventional-changelog：

![image-20210723145249096](https://tva1.sinaimg.cn/large/008i3skNgy1gsqvz2odi4j30ek00zmx2.jpg)

并且在 package.json 中进行配置：

![](https://tva1.sinaimg.cn/large/008i3skNgy1gsqvzftay5j30iu04k74d.jpg)

这个时候我们提交代码需要使用 `npx cz`：

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

### 代码提交验证

如果我们按照 cz 来规范了提交风格，但是依然有同事通过 `git commit` 按照不规范的格式提交应该怎么办呢？

- 我们可以通过 commitlint 来限制提交；

  1.安装 @commitlint/config-conventional 和 @commitlint/cli

```shell
npm i @commitlint/config-conventional @commitlint/cli -D
```

2.在根目录创建 commitlint.config.js 文件，配置 commitlint

```js
module.exports = {
  extends: ["@commitlint/config-conventional"],
};
```

3.使用 husky 生成 commit-msg 文件，验证提交信息：

```shell
npx husky add .husky/commit-msg "npx --no-install commitlint --edit $1"
```

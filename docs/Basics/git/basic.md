# Git

## 一、版本控制的起源

现在的软件项目通常是由一个研发小组共同分析、设计、编码、维护以及测试的

针对团队开发需要解决以下问题：

1.  备份多个版本，费空间，费时间
2.  难于恢复至以前正确版本
3.  容易引发 BUG
4.  解决代码冲突困难
5.  代码管理混乱
6.  难于追溯问题代码的修改人和修改时间
7.  无法进行权限控制
8.  项目版本发布困难

源代码管理工具就是为了解决上述问题应运而生的

## 二、版本控制

版本控制是维护工程蓝图的标准做法，能追踪工程蓝图从诞生一直到定案的过程。是一种记录若干文件内容变化，以便将来查阅特定版本修订情况的系统

如果是单人开发，也强烈建议现在就开始使用版本控制!

如果是开发团队中的一员，使用版本控制是强制性的！

使用版本控制可以：

- 不会对现有工作造成任何损害
- 不会增加工作量
- 添加新的功能拓展时，会变得更加容易

## 三、常见版本控制工具

1.  CVS 开启版本控制之门

- CVS 1990 年诞生，远古时代的主流源代码管理工具

2.  SVN 集中式版本控制之王者

- SVN:又称 subversion，是 CVS 的接班人，是一款**集中式**源代码管理工具。曾经是绝大多数开源软件的代码管理工具(google code)，前几年在国内软件企业使用最为普遍

3.  GIT 分布式版本控制之伟大作品

- GIT:一款**分布式**源代码管理工具，目前国内企业几乎都已经完成了从 SVN 到 GIT 的转换

集中式源代码管理

![](https://upload-images.jianshu.io/upload_images/647982-0038292fc7901b3c.png?imageMogr2/auto-orient/strip|imageView2/2/w/601/format/webp)

分布式源代码管理

![](https://upload-images.jianshu.io/upload_images/647982-b32b9194c125ecbf.png?imageMogr2/auto-orient/strip|imageView2/2/w/459/format/webp)

分布式和集中式的最大区别在于：

1.  在集中式下, 开发者只能将代码提交到服务器, 在分布式下, 开发者可以本地提交
2.  在集中式下, 只有远程服务器上有代码数据库, 在分布式下, 每个开发者机器上都有一个代码数据库

SVN(集中式)

![](https://upload-images.jianshu.io/upload_images/647982-04a8ecee2ca8e5c4.png?imageMogr2/auto-orient/strip|imageView2/2/w/744/format/webp)

GIT(分布式)

![](https://upload-images.jianshu.io/upload_images/647982-6a9d0974b77621bc.png?imageMogr2/auto-orient/strip|imageView2/2/w/483/format/webp)

## 四、git 简介

GIT 是一款自由和开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目

在世界上所有的分布式版本控制工具中，git 是最快、最简单、最流行的

是 Linux 之父李纳斯的第二个伟大作品

1.  2005 年由于 BitKeeper 软件公司对 Linux 社区停止了免费使用权。
2.  Linus 为了辅助 Linux 内核的开发(管理源代码),迫不得己自己开发了一个分布式版本控制工具，从而 Git 诞生了

## 五、git 工作原理

如果想学好 GIT 必须先了解 GIT 的工作原理

1.  **工作区**(Working Directory): 仓库文件夹里面, 除了`.git`目录以外的内容

2.  **版本库**(Repository): .git 目录, 用于存储记录版本信息

- 版本库中的暂缓区(staga):
- 版本库中的分支(master): git 自动创建的第一个分支
- 版本库中的 HEAD 指针:用于指向当前分支

3.  git add 和 git commit 命名作用

  git add: 把文件修改添加到暂缓区
  ![](https://upload-images.jianshu.io/upload_images/647982-143ae6cdb18d3212.png?imageMogr2/auto-orient/strip|imageView2/2/w/729/format/webp)

  git commit: 把暂缓区的所有内容提交到当前 HEAD 指针指向的分支
  ![](https://upload-images.jianshu.io/upload_images/647982-55c339632d9e540f.png?imageMogr2/auto-orient/strip|imageView2/2/w/775/format/webp)


## 六、安装

打开 Git 官网：[https://git-scm.com/](https://git-scm.com/)下载 git 对应操作系统的版本。

![image.png](https://img11.360buyimg.com/ddimg/jfs/t1/200493/25/5164/171158/6129fa35Eb7972819/9babd1c081443bb6.png)

官网下载太慢，我们可以使用淘宝镜像下载：[http://npm.taobao.org/mirrors/git-for-windows/](http://npm.taobao.org/mirrors/git-for-windows/)

下载对应的版本即可安装！

安装：无脑下一步即可！安装完毕就可以使用了！

安装好了以后在 cmd 输入`git --version`检查是否安装成功

![image.png](https://img12.360buyimg.com/ddimg/jfs/t1/199547/4/5152/5236/6129fa20E603806e5/a59cfc9c2f80ca65.png)

安装好了以后，在文件夹里面或者在桌面可以点击鼠标右键就会看到 Git 的两个程序

1.  **Git Bash**：Unix 与 Linux 风格的命令行，使用最多，推荐最多
2.  **Git GUI**：图形界面的 Git，不建议初学者使用，尽量先熟悉常用命令

## 七、环境配置

一般情况下我们会首先对 Git 进行一个配置

所有的配置文件，其实都保存在本地！

查看配置`git congif -l`

![image.png](https://img12.360buyimg.com/ddimg/jfs/t1/200027/37/5264/33506/612a4503E57be20bb/607603328d711765.png)

查看不同级别的配置文件：

```shell
# 查看系统config
git config --system --list
# 查看当前系统（global）配置
git config --global --list
```

Git 相关的配置文件：

1.  Git\etc\gitconfig ：Git 安装目录下的 gitconfig --system 系统级
2.  C:\Users\Administrator\ .gitconfig 只适用于当前登录用户的配置 --global 全局

![image.png](https://img10.360buyimg.com/ddimg/jfs/t1/189817/35/20337/6664/612a45c8E9544473d/d6df05b463c9abec.png)

这里可以直接编辑配置文件，通过命令设置后会响应到这里。

!>设置用户名与邮箱（用户标识，必要）

当你安装 Git 后首先要做的事情是设置你的用户名称和 e-mail 地址。这是非常重要的，因为每次 Git 提交都会使用该信息。它被永远的嵌入到了你的提交中：

```shell
git config --global user.name "xx"  #名称
git config --global user.email "xx"   #邮箱
```

只需要做一次这个设置，如果你传递了--global 选项，因为 Git 将总是会使用该信息来处理你在系统中所做的一切操作。如果你希望在一个特定的项目中使用不同的名称或 e-mail 地址，你可以在该项目中运行该命令而不要--global 选项。总之--global 为全局配置，不加为某个项目的特定配置。


## 八、Git 单人开发

首先我们会项目的根目录通过`Git init`仓库初始化（个人仓库）

之后就可以在编写我们的代码，仓库中有文件发生改动时

我们可以通过`git status`查看文件的状态

- 查看某个文件的状态：`git status 文件名`
- 查看当前路径所有文件的状态：`git status`

编写好代码后，可以通过`git add`将工作区的代码保存到暂缓区

- 保存某个文件到暂缓区：`git add 文件名`
- 保存当前路径的所有文件到暂缓区：`git add .`（注意，最后是一个点 . ）

之后我们可以通过`git commit`将暂缓区的文件提交到当前分支

- 提交某个文件到分支：`git commit -m ”本次提交描述”` 文件名
- 保存当前路径的所有文件到分支：`git commit -m ”本地提交描述”`

之后我们可以通过`git log`查看文件的修改日志

- 查看某个文件的修改日志`git log` 文件名
- 查看当前路径所有文件的修改日志：`git log`
- 查看最近的 N 次修改：`git log –N`（N 是一个整数）

我们也可以通过`git diff`查看文件最新改动的地方

- 查看某个文件的最新改动的地方：`git diff 文件名`
- 查看当前路径所有文件最新改动的地方：`git diff`

如果有想要删除的文件，可以通过`git rm`删除文件（删完之后要进行 commit 操作，才能同步到版本库）

当然有些情况我们想反悔，也可以进行`git reset`版本回退（建议加上––hard 参数，git 支持无限次后悔）

- 回退到上一个版本：`git reset ––hard HEAD^`
- 回退到上上一个版本：`git reset ––hard HEAD^^`
- 回退到上 N 个版本：`git reset ––hard HEAD~N`（N 是一个整数）
- 回退到任意一个版本：`git reset ––hard 版本号`（版本号用 7 位即可）

如果有些文件，我们不想让 git 进行管理，想要忽略它，也可以在`.gitignore`文件中进行配置

```
#               表示此为注释,将被Git忽略
*.a             表示忽略所有 .a 结尾的文件
!lib.a          表示但lib.a除外
/TODO           表示仅仅忽略项目根目录下的 TODO 文件，不包括 subdir/TODO
build/          表示忽略 build/目录下的所有文件，过滤整个build文件夹；
doc/*.txt       表示会忽略doc/notes.txt但不包括 doc/server/arch.txt

bin/:           表示忽略当前路径下的bin文件夹，该文件夹下的所有内容都会被忽略，不忽略 bin 文件
/bin:           表示忽略根目录下的bin文件
/*.c:           表示忽略cat.c，不忽略 build/cat.c
debug/*.obj:    表示忽略debug/io.obj，不忽略 debug/common/io.obj和tools/debug/io.obj
**/foo:         表示忽略/foo,a/foo,a/b/foo等
a/**/b:         表示忽略a/b, a/x/b,a/x/y/b等
!/bin/run.sh    表示不忽略bin目录下的run.sh文件
*.log:          表示忽略所有 .log 文件
config.php:     表示忽略当前路径的 config.php 文件

/mtk/           表示过滤整个文件夹
*.zip           表示过滤所有.zip文件
/mtk/do.c       表示过滤某个具体文件

被过滤掉的文件就不会出现在git仓库中（gitlab或github）了，当然本地库中还有，只是push的时候不会上传。

需要注意的是，gitignore还可以指定要将哪些文件添加到版本管理中，如下：
!*.zip
!/mtk/one.txt

唯一的区别就是规则开头多了一个感叹号，Git会将满足这类规则的文件添加到版本管理中。为什么要有两种规则呢？
想象一个场景：假如我们只需要管理/mtk/目录中的one.txt文件，这个目录中的其他文件都不需要管理，那么.gitignore规则应写为：：
/mtk/*
!/mtk/one.txt

假设我们只有过滤规则，而没有添加规则，那么我们就需要把/mtk/目录下除了one.txt以外的所有文件都写出来！
注意上面的/mtk/*不能写为/mtk/，否则父目录被前面的规则排除掉了，one.txt文件虽然加了!过滤规则，也不会生效！

----------------------------------------------------------------------------------
还有一些规则如下：
fd1/*
说明：忽略目录 fd1 下的全部内容；注意，不管是根目录下的 /fd1/ 目录，还是某个子目录 /child/fd1/ 目录，都会被忽略；

/fd1/*
说明：忽略根目录下的 /fd1/ 目录的全部内容；

/*
!.gitignore
!/fw/
/fw/*
!/fw/bin/
!/fw/sf/
说明：忽略全部内容，但是不忽略 .gitignore 文件、根目录下的 /fw/bin/ 和 /fw/sf/ 目录；注意要先对bin/的父目录使用!规则，使其不被排除。
```

工作流程

**准备工作**

1.  创建一个工作区
2.  在工作区中打开 git 终端
3.  通过`git init`指令,初始化版本库
4.  通过`git config user.name "用户名"`,`git config user.email "邮箱"`设置用户名和邮箱（一般情况下设置了全局的话可以不必再设置，有特殊的需求可以设置局部的用户名和邮箱）
5.  通过`git config -l`查看设置情况

**开发阶段**

1.  编写代码
2.  通过`git add 文件名称/git add .`添加到版本库的暂缓区中
3.  通过`git commit -m "描述"`将暂缓区的文件添加到 HEAD 指针指向的分支中
    （默认只有一个分支，master 分支，也称之为主分支）

注意点：

1.  不是写一句代码就 add commit 一次，应该是完成一个功能后再 add commit
2.  commit 时-m 提交描述一定要认真编写，与当前提交内容保持一致，否则容易挨骂

**单人使用 Git 管理项目的好处**

1.  可以通过`git log / git reflog`查看项目演变历史
2.  可以通过`git reset --hard 版本号`在任意版本之间切换
3.  无需备份多个文件，每次 commit 提交 Git 会自动备份


## 九、多人开发

**基础指令**

- `git init --bare` : 仓库初始化(共享仓库)
- 注意: 不要直接在共享仓库中编写代码
- `git clone`：下载远程仓库到本地
  - 下载远程仓库到当前路径：git clone 仓库的 URL
  - 下载远程仓库到特定路径：git clone 仓库的 URL 存放仓库的路径
- `git pull`：下载远程仓库的最新信息到本地仓库
- `git push`：将本地的仓库信息推送到远程仓库
  - 提交时如果远程仓库有其它人提交的最新代码, 必须先 pull, 再提交

**冲突解决**

当多个人同时修改了同一个文件时, 后提交的需要先从服务器 pull 代码到问题, 手动解决完冲突之后再 push 到远程服务器

**分支的使用**

- 如何创建一个分支？

  1.  可以通过`git branch`指令查看当前的仓库的分支
  2.  可以通过`git branch 分支名称`来创建一个新的分支
  3.  可以通过`git switch 分支名称`来切换分支

- 如何删除分支

  1.  可以通过`git branch -d 分支名称`指令删除指定的分支

      注意点：以上指令仅仅只是删除了本地的分支，并不会删除远程服务器上的分支

  2.  可以通过`git push origin --delete 分支名称`指令删除远程服务器指定的分支

- 如何合并分支

  1.  可以通过`git merge 分支名称`指令来合并

      比如：我们在 master 分支中使用 git merge Dev 就代表需要将 dev 分支上的代码合并到 master 分支

- 如何将分支提交到远程服务器

  1.  可以通过 git branch -r 指令查看远程仓库有哪些分支
  2.  首先切换到新建的分支，然后在新建的分支中通过`git push --set-upstream origin 分支名称`

**工具**

实际开发中，一步一步敲命令是很繁琐且效率很低，一般情况下我们会使用插件来进行辅助开发

这里我就推荐 vscode 的**GitLens — Git supercharged**插件

![image.png](https://img14.360buyimg.com/ddimg/jfs/t1/199579/3/5441/67477/612b99cbE48573f6a/9017cba21a668850.png)

具体如何使用这个插件，可以自行搜索，这里我就不一一赘述了

📚 最后推荐一些我认为自学 Git 不错的资源

- Git 教程-廖雪峰出品：[https://www.liaoxuefeng.com/wiki/896043488029600](https://www.liaoxuefeng.com/wiki/896043488029600)
- 狂神说 Git 最新教程通俗易懂：[https://www.bilibili.com/video/BV1FE411P7B3?from=search&seid=16292658412211606618](https://www.bilibili.com/video/BV1FE411P7B3?from=search&seid=16292658412211606618)

!> 关于 GitHub 和 Gitflow 工作流很简单，请自行搜索教程，这里我就不写教程了

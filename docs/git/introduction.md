## GIT 简介

GIT 是一款自由和开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目

在世界上所有的分布式版本控制工具中，git 是最快、最简单、最流行的

是 Linux 之父李纳斯的第二个伟大作品

1.  2005 年由于 BitKeeper 软件公司对 Linux 社区停止了免费使用权。
2.  Linus 为了辅助 Linux 内核的开发(管理源代码),迫不得己自己开发了一个分布式版本控制工具，从而 Git 诞生了

## GIT 工作原理

如果想学好 GIT 必须先了解 GIT 的工作原理

1.  **工作区**(Working Directory): 仓库文件夹里面, 除了`.git`目录以外的内容

2.  **版本库**(Repository): .git 目录, 用于存储记录版本信息

- 版本库中的暂缓区(staga):
- 版本库中的分支(master): git 自动创建的第一个分支
- 版本库中的 HEAD 指针:用于指向当前分支

3.  git add 和 git commit 命名作用

- git add: 把文件修改添加到暂缓区
  ![](https://upload-images.jianshu.io/upload_images/647982-143ae6cdb18d3212.png?imageMogr2/auto-orient/strip|imageView2/2/w/729/format/webp)
- git commit: 把暂缓区的所有内容提交到当前 HEAD 指针指向的分支
  ![](https://upload-images.jianshu.io/upload_images/647982-55c339632d9e540f.png?imageMogr2/auto-orient/strip|imageView2/2/w/775/format/webp)



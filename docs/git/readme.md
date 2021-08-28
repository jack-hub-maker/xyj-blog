## 版本控制的起源

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

## 版本控制

版本控制是维护工程蓝图的标准做法，能追踪工程蓝图从诞生一直到定案的过程。是一种记录若干文件内容变化，以便将来查阅特定版本修订情况的系统

如果是单人开发，也强烈建议现在就开始使用版本控制!

如果是开发团队中的一员，使用版本控制是强制性的！

使用版本控制可以：

- 不会对现有工作造成任何损害
- 不会增加工作量
- 添加新的功能拓展时，会变得更加容易

## 常见版本控制工具

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

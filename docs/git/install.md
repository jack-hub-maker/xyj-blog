## 安装

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

## 环境配置

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

> 设置用户名与邮箱（用户标识，必要）

当你安装 Git 后首先要做的事情是设置你的用户名称和 e-mail 地址。这是非常重要的，因为每次 Git 提交都会使用该信息。它被永远的嵌入到了你的提交中：

```shell
git config --global user.name "xx"  #名称
git config --global user.email "xx"   #邮箱
```

只需要做一次这个设置，如果你传递了--global 选项，因为 Git 将总是会使用该信息来处理你在系统中所做的一切操作。如果你希望在一个特定的项目中使用不同的名称或 e-mail 地址，你可以在该项目中运行该命令而不要--global 选项。总之--global 为全局配置，不加为某个项目的特定配置。

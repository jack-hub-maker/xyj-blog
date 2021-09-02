# 多人开发

> 基础指令

- `git init --bare` : 仓库初始化(共享仓库)
- 注意: 不要直接在共享仓库中编写代码
- `git clone`：下载远程仓库到本地
  - 下载远程仓库到当前路径：git clone 仓库的 URL
  - 下载远程仓库到特定路径：git clone 仓库的 URL 存放仓库的路径
- `git pull`：下载远程仓库的最新信息到本地仓库
- `git push`：将本地的仓库信息推送到远程仓库
  - 提交时如果远程仓库有其它人提交的最新代码, 必须先 pull, 再提交

> 冲突解决

当多个人同时修改了同一个文件时, 后提交的需要先从服务器 pull 代码到问题, 手动解决完冲突之后再 push 到远程服务器

> 分支的使用

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

> 工具

实际开发中，一步一步敲命令是很繁琐且效率很低，一般情况下我们会使用插件来进行辅助开发

这里我就推荐 vscode 的**GitLens — Git supercharged**插件

![image.png](https://img14.360buyimg.com/ddimg/jfs/t1/199579/3/5441/67477/612b99cbE48573f6a/9017cba21a668850.png)

具体如何使用这个插件，可以自行搜索，这里我就不一一赘述了

📚 最后推荐一些我认为自学 Git 不错的资源

- Git 教程-廖雪峰出品：[https://www.liaoxuefeng.com/wiki/896043488029600](https://www.liaoxuefeng.com/wiki/896043488029600)
- 狂神说 Git 最新教程通俗易懂：[https://www.bilibili.com/video/BV1FE411P7B3?from=search&seid=16292658412211606618](https://www.bilibili.com/video/BV1FE411P7B3?from=search&seid=16292658412211606618)

> [!tip]
> 关于 GitHub 和 Gitflow 工作流很简单，请自行搜索教程，这里我就不写教程了

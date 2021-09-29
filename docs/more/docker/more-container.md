## docker 多容器的操作

相信通过上节的学习，小伙伴们已经对容器的创建、查看和删除命令有了认识。这节我们讲一下对于多个容器的操作。因为在实际工作中，我们往往管理的都是多个容器，这也是容器的主要特点之一。

## 创建多个容器

在 WIndows 环境下我们来作这个，先打开三个 PowerShell 窗口，然后在每个窗口中输入创建容器的命令，这里以`Nginx`镜像为例。

```shell
docker run nginx
```

然后再重新打开一个 PowerShell 窗口，输入查看命令，查看已经开启的容器。

```shell
docker ps
```

可以看到现在已经有 3 个开启的容器了。
![image.png](https://img14.360buyimg.com/ddimg/jfs/t1/179909/8/19134/22105/61172a28E58abc18b/10279adedd92daca.png)

## 停止多个容器

现在要把三个容器用一个命令停掉，笨的方法是直接加上 ID 或名字。

```shell
docker stop [id1,id2,id3...]
```

但如果你想想，比如这时候有 100 个容器，我们用这种方法就会非常麻烦。

这时候我们需要学一个新的查看命令，比如只查看现在所有容器的 ID,命令如下。

```shell
docker ps -aq
```

这样就打印出了所有容器的 ID，这时候包括没有开启的。

有了这个命令之后，我们就可以作一个命令组合。

```shell
docker stop $(docker ps -aq)
```

命令执行后，会返回给我们容器的编号，说明已经停止了。可以使用下面的命令再次查看。

```shell
docker ps -a
```

这时候就可以看到，所有容器不在是 up 状态了，而是 exited 状态。
![image.png](https://img14.360buyimg.com/ddimg/jfs/t1/183160/20/19206/22971/61172ae8E9b5fd778/b29295ffe1b27cfa.png)

## 删除多个容器

会了停止多个容器，那删除多个容器就很简单了。

```shell
docker rm $(docker ps -aq)
```

## 强制删除容器

正在运行的容器，是不可以直接删除的，会报错。我们来做个实验。新建一个容器.

```shell
docker run nginx
```

然后新开一个 PowerShell，直接使用 rm 命令删除。

```shell
docker rm [id/image name]
```

这时候会直接报错。报错内容如下。

![image.png](https://img13.360buyimg.com/ddimg/jfs/t1/187494/20/18428/25563/61172b94E28309dc2/e69f284260659383.png)

报错信息大体就是不能删除没有 stop 的容器。但这时候就是要删除，你可以使用强制删除命令进行删除。

```shell
docker rm [id/image name] -f
```

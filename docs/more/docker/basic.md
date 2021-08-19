安装完了以后，我们先来学习一下 docker 容器的相关的命令

我们先来了解一下镜像和容器的区别

## 镜像和容器的区别

镜像类似于虚拟机的模板，可以理解为面向对象中的只读类

容器和镜像几乎一模一样，唯一的区别是镜像是只读的，而容器上面有一个可读写层。所以容器=镜像+**读写层**。

![https://newimg.jspang.com/Docker4_ContainerAndImages.jpg](https://newimg.jspang.com/Docker4_ContainerAndImages.jpg)

## 增

当你明白了什么是镜像和容器后，我们一起试着来创建一个容器。创建容器的命令是。

```shell
docker container run [image name]
# 简写
docker run [image name]
```

image 代表一个镜像的名称，如果你想使用的镜像名称是 nginx，就可以写成下面的样子。

```shell
docker run nginx
```

输入完成后，直接回车。如果系统中没有这个镜像，Docker 会自动去`Docker Hub`上拉取对应的镜像到本地，然后执行对应的`Shell`脚本，脚本会把镜像自动安装到 Doker 容器里，并最终启动对于的镜像服务。

> Docker Hub 是 Docker 官方的镜像和社区，里边有很多开发者制作好的镜像，我们可以直接使用这些镜像。如果你有能力，也可以制作镜像，并上传到 Docker Hub。

## 改

因为改的内容非常多，这里暂时先不学习改的知识，留在后面讲

## 查

创建完容器后，如果查看这个容器的信息和状态喃？这时候你可以使用下面的命令。

!>注意你这时候需要新打开一个 PowerShell/cmd 窗口，再执行命令,因为刚才创建容器的时候，默认会把容器的服务开启的

```shell
docker container ls
# 这个命令是没有简写的
```

> [!tip]
> 默认情况下查看容器的时候，只会显示开启了服务的容器

如果想要查看全部的容器,可以在后面可以加一个-a

```shell
docker container ls -a
# 这个命令是没有简写的
```

注意你这时候需要新打开一个 PowerShell 窗口，再执行命令

1.  CONTAINER ID : 容器对应的 ID，这个是唯一的
2.  IMAGE : 使用的镜像名称，显示不同
3.  COMMAND : 执行的相关命令
4.  CREATED: 创建的时间
5.  STATUS: 目前镜像的状态，一般会有两种状态 Up 和 Exited.
6.  PORTS: 协议和端口
7.  NAMES: 容器的名称，名字是 Docker 随机生成的

还有一种查看容器的命令，不过这是老版本的命令，**不建议**使用

```shell
docker container ps
# 简写
docker ps
```

## 删

如果你想停止掉一个正在运行的容器，可以使用下面的命令：

```shell
docker container stop [name/ID]
# 简写
docker stop [name/ID]
```

可以输入容器的名称或者 id，一般我们会选择输入 id 的前三位(比较方便)

当我们停止容器之后，容器并没有删除，而只是停止掉了。这时候你可以使用下面的命令删除容器。

```shell
docker container rm [name/ID]
# 简写
docker rm [name/ID]
```

## 演练

我就在 Windows 上来进行演练

首先打开 docker 服务

之后我们来查看一下是否开启了服务

```shell
docker version
```

![image.png](https://img12.360buyimg.com/ddimg/jfs/t1/198139/19/2955/48084/61160cd9Ebef038dc/52809a873e8062f0.png)

先创建一个 nginx 容器

```shell
docker container run nginx
```

![image.png](https://img12.360buyimg.com/ddimg/jfs/t1/186829/20/18037/97083/61160d0dE0cfa33f5/af7ad4e2df69006b.png)

另外打开一个新的窗口,查看是否安装成功

```shell
docker container ls
```

![image.png](https://img13.360buyimg.com/ddimg/jfs/t1/205516/25/1073/12062/61160d62Eb2b41cea/f4eb5903b3a84213.png)

我们再来安装一个 ubuntu 容器

```shell
docker container run ubuntu
```

默认 ubuntu 容器安装完之后是没有开启服务的

我们来查看一下

```shell
docker ps
```

![image.png](https://img12.360buyimg.com/ddimg/jfs/t1/196341/1/18083/13169/61160ee2E6b123c5e/3c47788cfd60f61c.png)

这个时候我们就可以添加-a 来查看所有容器

```shell
docker ps -a
```

![image.png](https://img13.360buyimg.com/ddimg/jfs/t1/185967/3/19013/17674/61160f1dE8710862a/e3b719fb7e5add2e.png)

接下来我们来停止 nginx 服务

我一般改和删我都是选择容器的前三位 id 来

```shell
docker stop 0f2
```

![image.png](https://img13.360buyimg.com/ddimg/jfs/t1/188417/1/18087/6636/61160f7fE862824b0/a61bdfd32ad3b251.png)

这样 docker 就没有开启任何服务了

接下来我们把 ubuntu 删掉

```shell
docker rm bea
```

![image.png](https://img12.360buyimg.com/ddimg/jfs/t1/189902/5/18145/3537/61160fbeEe8c5e74f/d18e988c5703bcfe.png)

来查看一下是否删除成功

```shell
docker ps -a
```

![image.png](https://img11.360buyimg.com/ddimg/jfs/t1/184893/11/19046/12499/61161023E9ed0095f/c2ad91defb8133c0.png)

这样容器的基本操作就演练完了

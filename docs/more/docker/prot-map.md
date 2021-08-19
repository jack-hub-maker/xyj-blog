这节学习一下 Docker 的端口映射和两种运行模式-attached 和 detached 模式。先来看如何把一个容器的端口映射到主机的 80 端口上。

## 端口映射

在开启端口映射之前，你首先要之道 Docker 对应的容器端口是多少。比如 Nginx 镜像的端口是 80。知道这个端口后，就可以在启动容器的时候，用-p `port:port`的形式，启用映射了。

用 Nginx 举例:

```shell
docker run -p 90:80 nginx
```

等待项目启动后，打开浏览器窗口，在地址栏输入 127.0.0.1:90，就可以打开 nginx 的默认网址。

![image.png](https://img10.360buyimg.com/ddimg/jfs/t1/189379/14/18286/42205/61188531E9c07d346/7b1429e3bf3109d5.png)

第一个端口是映射到服务器本机的端口;第二个端口是 Docker 容器使用的端口。 比如你想把 Docker 的 80 端口，映射到服务器的 90 端口。

## 模式

### attached

两种模式最简单的对比理解就是：`attached`模式在前台运行，`detached`模式在后台运行。

当你打开 127.0.0.1 网址的时候，PowerShell 上打印出了相关的日志（log），并且每访问一次，都会增加一条日志。也就是说 Docker 容器的日志会实时的展现到窗口并且占用此端口。这种模式叫做 attached 模式。

![image.png](https://img11.360buyimg.com/ddimg/jfs/t1/187679/36/18342/46039/611885ffE7056cbef/cbaa43a4f4b32bef.png)

在 windows 系统下并不是一个完整的 attached 模式，只是帮我们打印出了 Log,并且不容易停止服务

到 Linux 服务器上，这时候你按 Ctrl+C,就会停止掉 Docker 服务。而现实中我们工作的环境恰恰是这种 Linux 环境。

```shell
docker run -p 100:80 nginx
```

![image.png](https://img11.360buyimg.com/ddimg/jfs/t1/185042/5/19444/44557/611887e1Eb150ee3c/a3c8fbf25f51054d.png)

![image.png](https://img14.360buyimg.com/ddimg/jfs/t1/180285/7/18846/145464/61188828E317d266c/6a951d3bc2d2b1f1.png)

也就是在 Linux 上你的操作命令，会直接传递个 Docker 容器。这个缺点就是很容易误操作，比如在公司的生产环境中，你直接一个 Ctrl+C，整个服务就崩掉了，你这个月的绩效也就没有了。

所以我们需要一个更好的，更稳定的模式。也就是 detached 模式。attached 模式更适用于容器和程序的调试阶段。

### detached

detached 模式的开启方法，就是加一个参数-d 或者--detach。

```shell
docker run -d -p 80:80 nginx
```

![image.png](https://img13.360buyimg.com/ddimg/jfs/t1/199350/16/3044/16773/61188984Ed708c8c6/f995dda7e1d0c972.png)

这次你会看到，和 attached 模式不同的是，这次输入完命令后，只显示出了容器的编号，并且可以再输入任何命令。就算我们关掉窗口，容器依然运行，也就是他是在系统后台进行运行的。

这种就比较适合在生产环境中运行，停掉和删除容器都需要使用`Shell`脚本的形式。减少了很多误操作。

### detached 转换 attached 模式

在运行之后，也有需要调试的时候，Docker 提供了两个模式间的转换。比如现在要把 detached 模式的容器，改为 attched 模式。‘

![image.png](https://img14.360buyimg.com/ddimg/jfs/t1/181348/31/19238/14181/61188b0bE93dc9458/d45f93ea345acc2f.png)

### detached模型下查看日志

```shell
docker run -d -p 80:80 nginx
```

容器被运行起来了，是detached模式，也就是Docker 的后台运行模式。这时候想要查看后台日志，可以使用下面的命令查看。

```shell
docker container logs [id/image name]
```
![image.png](https://img11.360buyimg.com/ddimg/jfs/t1/191652/26/18580/71775/6119dbd9E201238d9/d037df35bdd07b99.png)

虽然日志在窗口中出现了，但只打印一次logs,如果想动态一直跟踪日志，可以在命令上加入一个-f。

```shell
docker logs -f [id/image name]
```

输入完上面的命令，打开浏览器，在地址栏输入127.0.0.1，也就是访问本地的nginx服务。你会看到日志窗口就会跟踪到最新的日志。

![image.png](https://img14.360buyimg.com/ddimg/jfs/t1/183003/26/19359/118759/6119dc1eE4f284690/4b8afb83d9a49b77.png)

如果想关闭日志跟踪模式，直接用快捷键Ctrl+C就可以结束掉了。
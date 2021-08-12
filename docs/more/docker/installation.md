安装docker很简单
1.  进入官网[https://www.docker.com/get-started](https://www.docker.com/get-started)
2.  根据自己的操作系统进行安装

## Windows

直接点击安装即可，安装过程一直下一步即可

安装成功打包docker,等待一会,当显示这个界面的时候代表安装成功

![Snipaste_2021-08-11_14-38-56.png](https://img12.360buyimg.com/ddimg/jfs/t1/192121/20/17713/50802/61137095E2351dc0a/54341b5f74619cab.png)

接下来在cmd中或者powershell中输入

```shell
docker version
```

这样就代表在Windows下安装docker成功了

![Snipaste_2021-08-11_14-41-27.png](https://img13.360buyimg.com/ddimg/jfs/t1/196622/15/17609/38399/61137126E9232f68f/13a997ee2e6d36ba.png)

## Linux

安装linux版本的docker可以选择虚拟机或者服务器上安装

这里我就以服务器来进行演示

首先

```shell
curl -fsSL get.docker.com -o get-docker.sh
```

然后查看是否安装成功

```shell
ls
```

如果目录下有`get-docker.sh`文件就说明安装成功

然后我们来执行这个文件

```shell
sudo sh get-docker.sh
```

之后就等待安装成功，时间稍微来说有点长，几分钟的样子

安装完毕之后我们来查看docker是否安装成功

```shell
docker version
```

![image.png](https://img13.360buyimg.com/ddimg/jfs/t1/206103/21/1023/30114/61153e02Ef9e184b0/b55d178d9aca7284.png)

我们会看到默认只开启了客户端,想开启服务端的话，需要输入

```shell
sudo systemctl start docker
```

好了以后,我们再来查看一下试试

```shell
docker version
```

![image.png](https://img12.360buyimg.com/ddimg/jfs/t1/187868/30/17984/83639/61153e5cE601f280f/3cf9dbd6bd3e85d7.png)

这样我们的linux下安装docker也就没有问题了
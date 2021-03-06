# Docker 学习笔记 - Docker 核心概念与安装


## 一、Docker 三大核心概念

Docker 三大核心概念如下，只有理解了这三个核心概念，才能顺利的理解 Docker 容器的整个生命周期。

#### 1. 镜像（Image）
Docker 镜像类似于虚拟机镜像，可以将它理解为一个只读的模板。例如，一个镜像可以包含一个基本的操作系统环境，里面仅安装了 Apache 应用程序（或用户需要的其他软件）。可以把它称为一个 Apache 镜像。

#### 2. 容器（Container）
Docker 容器类似于一个轻量级的沙箱，Docker 利用容器来运行和隔离应用。简单来说，容器是镜像的一个运行实例。所不同的是，镜像是静态的只读文件，而容器带有运行时需要的可写文件层。

#### 3. 仓库（Repository）
Docker 仓库类似于代码仓库，它是 Docker 集中存放镜像文件的场所。


## 二、安装 Docker

#### 1. 版本选择
从 [Docker 官网](https://www.docker.com) 上可以看到，目前 Docker 分 CE 和 EE 两种，每种支持的平台也略有不同，大家可以根据自己的需要进行选择，学习选择 CE 版本即可。详情如下图：

![docker-ce-ee.jpg](http://www.mayanlong.com/usr/uploads/2017/10/1818953991.jpg)

#### 2. 安装

安装很简单，大家到 [Get Docker](https://www.docker.com/get-docker) 页面下载对应平台的安装包安装即可。需要注意的是 macOS 10.10.3 之前版本、windows7和8 需要通过虚拟机方式来支持，下载对应平台的 [Docker Toolbox](https://www.docker.com/products/docker-toolbox) 安装即可。 

#### 3. 检测是否安装成功
打开终端，执行 `docker version`，如出现如下信息则安装成功：
```
$ docker version
Client:
 Version:      17.09.0-ce
 API version:  1.32
 Go version:   go1.8.3
 Git commit:   afdb6d4
 Built:        Tue Sep 26 22:40:09 2017
 OS/Arch:      darwin/amd64

Server:
 Version:      17.09.0-ce
 API version:  1.32 (minimum version 1.12)
 Go version:   go1.8.3
 Git commit:   afdb6d4
 Built:        Tue Sep 26 22:45:38 2017
 OS/Arch:      linux/amd64
 Experimental: true

```


## 三、相关链接

[Docker 官网：https://www.docker.com](https://www.docker.com)<br>
[Get Docker：https://www.docker.com/get-docker](https://www.docker.com/get-docker)<br>
[Docker Toolbox：https://www.docker.com/products/docker-toolbox](https://www.docker.com/products/docker-toolbox)

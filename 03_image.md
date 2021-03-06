# Docker 学习笔记 - Docker 镜像命令汇总


## 一、概念

镜像是 Docker 三大核心概念中最为重要的，是运行容器的前提。

Docker 运行容器前需要本地存在对应的镜像，如果本地不存在该镜像，Docker 会尝试先从默认镜像仓库下载（默认使用 Docker hub 公共注册服务器中的仓库），用户也可以通过配置，使用自定义的镜像仓库。

下面总结了常用的镜像相关命令。


## 二、搜索镜像

docker search 命令可以搜索远端仓库中共享的镜像，默认搜索官方仓库中镜像，命令如下：
```
$ docker search [OPTIONS] TERM
```


## 三、 获取镜像

docker pull 命令如下：
```
$ docker pull [OPTIONS] NAME[:TAG|@DIGEST]
```

从 Docker hub 获取一个 ubuntu 镜像
```
$ docker pull ubuntu:latest
```

也可以从指定的非官方仓库下载，下面是从网易蜂巢下载 ubuntu 镜像
```
$ docker pull hub.c.163.com/library/ubuntu:latest
```


## 四、查看镜像

#### 1. 使用 images 命令列出镜像

可以看到下面列出了刚刚下载的两个镜像信息。
```
$ docker images
REPOSITORY                     TAG                 IMAGE ID            CREATED             SIZE
ubuntu                         latest              ccc7a11d65b1        2 months ago        120MB
hub.c.163.com/library/ubuntu   latest              ccc7a11d65b1        2 months ago        120MB
```

#### 2. 使用 tag 命令添加镜像标签

为了方便后续工作使用特定的镜像，可以使用 tag 命令来为本地镜像任意添加新的标签。例如为 ubuntu:latest 镜像添加一个 myubuntu:latest 镜像标签：
```
$ docker tag ubuntu:latest myubuntu:latest
```

#### 3. 使用 inspect 命令查看详细信息

docker inspect 命令可以获取镜像的详细信息，包括制作者、适用架构、各层的数字摘要等：
```
$ docker inspect ubuntu:latest
```

#### 4. 使用 history 命令查看镜像历史

镜像文件由多个层组成，使用 history 命令可以列出各层的创建信息：
```
$ docker history ubuntu:latest
```


## 五、删除镜像

删除镜像使用 docker rmi 命令，格式如下，其中 IMAGE 可以是标签或 Image ID：
```
$ docker rmi [OPTIONS] IMAGE [IMAGE...]
```

删除 ubuntu:latest 镜像 ：
```
$ docker rmi ubuntu:latest
```

注意，当有该镜像创建的容器存在时，镜像文件默认是无法被删除的。可以使用 -f 参数来强制删除一个存在容器依赖的镜像，但并不推荐。正确的做法是，先删除依赖该镜像的所有容器，再来删除镜像。

#### 演示删除存在容器的镜像
使用 ubuntu:latest 镜像创建一个简单的容器来输出一段话：
```
$ docker run ubuntu:latest echo 'hello'
```

使用 docker ps -a 命令可以看到本机上存在的所有容器，可以看到后台存在一个基于 ubuntu:latest 镜像创建的且处于退出状态的容器 bbb5ae0d3851：
```
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
bbb5ae0d3851        ubuntu              "echo hello"        22 seconds ago      Exited (0) 21 seconds ago                       unruffled_swartz
```

如果删除该镜像，Docker 会提示有容器正在运行，无法删除：
```
$ docker rmi ubuntu:latest
Error response from daemon: conflict: unable to remove repository reference "ubuntu:latest" (must force) - container bbb5ae0d3851 is using its referenced image ccc7a11d65b1
```

正确的做法是，首先应该删除容器 bbb5ae0d3851，再删除镜像（ubuntu:latest 镜像ID 为 ccc7a11d65b1，此处用镜像ID删除）
```
$ docker rm bbb5ae0d3851
$ docker rmi ccc7a11d65b1
```


## 六、创建镜像

创建镜像的方法主要有三种：

#### 1. 基于已有镜像的容器创建
该方法主要使用 commit 命令，格式如下：
```
$ docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
```

#### 2. 基于本地模板导入
用户也可以直接从一个操作系统模板文件导入一个镜像，主要使用 import 命令，格式如下：
```
$ docker import [OPTIONS] file|URL|- [REPOSITORY[:TAG]]
```

#### 3. 基于 Dockerfile 创建
后面会单独总结


## 七、存出和载入镜像

#### 1. 存出镜像

导出镜像到本地文件，可以使用 docker save 命令，例如导出本地的 ubuntu:latest 镜像为文件 ubuntu_latest.tar
```
$ docker save -o ubuntu_latest.tar  ubuntu:latest
```
之后，用户可以通过 ubuntu_latest.tar 文件将该镜像分享给其他人。

#### 2. 载入镜像

可以使用 docker load 将导出的 tar 文件导入到本地镜像库，例如从文件 ubuntu_latest.tar 导入镜像到本地镜像列表：
```
$ docker load --input ubuntu_latest.tar 
```
或：
```
$ docker load < ubuntu_latest.tar 
```


## 八、上传镜像

可以使用 docker push 命令上传镜像到仓库，默认上传到 Docker Hub 官方仓库。

首先需要在 Docker Hub 注册账号，第一次上传需要登录，命令如下：
```
$ docker login
```

只能将镜像上传到自己的账号下面，比如我的用户名为 yanlongma，如果想上传本地的 ubuntu:latest 镜像，需要先添加新标签 yanlongma/ubuntu:latest，然后用 docker push 命令上传镜像：
```
$ docker tag ubuntu:latest yanlongma/ubuntu:latest
$ docker push yanlongma/ubuntu:latest
```

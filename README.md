
# ubuntu14.04 安装docker和简单实用

* 安装教程

https://github.com/doctording/chinese_docker/blob/master/installation/ubuntu.md

* 注册docker账号

https://cloud.docker.com


## hello world运行

![](./imgs/hello_1.png)

![](./imgs/hello_2.png)

## 使用镜像，在容器中运行
```
$docker run ubuntu:14.04 /bin/echo 'Hello world'
```
![](./imgs/run.png)

![](./imgs/run_2.png)

## 交互式的docker

```
$docker run -t -i ubuntu:14.04 /bin/bash
```

-t 表示在新容器内指定一个伪终端或终端，
-i 表示允许我们对容器内的 (STDIN) 进行交互。

```
john@ubuntu:~$ docker run -t -i ubuntu:14.04 /bin/bash
root@d424469ea86a:/# pwd
/
root@d424469ea86a:/# ls
bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
boot  etc  lib   media  opt  root  sbin  sys  usr
root@d424469ea86a:/# exit
exit
```
![](./imgs/docker_t_i.png)

---

## 守护进程

* 构建容器

```
$docker run -d ubuntu:14.04 /bin/sh -c "while true; do echo hello world; sleep 1; done"
```
-d 标识告诉 docker 在容器内以后台进程模式运行。

![](./imgs/daemon.png)

这个长的字符串叫做容器ID（container ID）。它对于每一个容器来说都是唯一的，所以我们可以使用它.

当命令运行之后，容器的状态随之改变并且被系统自动分配了名称 
modest_sammet

* 查看容器的标准输出

```
$docker logs modest_sammet
hello world
hello world
hello world
. . .
```
docker logs 命令会查看容器内的标准输出：这个例子里输出的是我们的命令 hello world

* 停止运行的容器
docker stop 命令会通知 Docker 停止正在运行的容器。如果它成功了，它将返回刚刚停止的容器名称。

![](./imgs/daemon_2.png)

---

# 镜像

## 管理和使用本地 Docker 主机镜像。

### 1 在主机上列出镜像列表($docker images)

![](./imgs/docker_images.png)

* 来自什么镜像源，例如 ubuntu
* 每个镜像都有标签(tags)，例如 14.04
* 每个镜像都有镜像ID

### 2 获取一个新的镜像($docker pull)

```
$ sudo docker pull centos
Pulling repository centos
b7de3133ff98: Pulling dependent layers
5cc9e91966f7: Pulling fs layer
511136ea3c5a: Download complete
ef52fb1fe610: Download complete
. . .
```

### 3 查找镜像($docker search)
![](./imgs/docker_search.png)

## 创建基本镜像

* 手动新建Dockerfile,并创建一个镜像
```
$ mkdir sinatra
$ cd sinatra
$ touch Dockerfile
```
Dockerfile内容如下
```
# This is a comment
FROM ubuntu:14.04
MAINTAINER Kate Smith <ksmith@example.com>
RUN apt-get update && apt-get install -y ruby ruby-dev
RUN gem install sinatra
```
* $docker build 新建
```
docker build -t doctording/sinatra:label_1 .
```
使用 docker build 命令并指定 -t 标识(flag)来标示属于 doctording ，镜像名称为 sinatra,标签是 label_1
. 当前目录

![](./imgs/docker_build.png)

![](./imgs/docker_build_3.png)

## 上传 Docker 镜像到 Docker Hub Registry

### 在提交更改和构建之后为镜像来添加标签($docker tag)

```
docker tag 2cb22eee3559 doctordingdocker/sinatra:devel
```

### 显示创建的镜像

```
docker images doctordingdocker/sinatra:devel
```

### push 到 hub.docker.com

* 首先需要docker login 认证
```
$docker login
```
输入自己注册的用户名，密码即可

* docker push
```
docker push doctordingdocker/sinatra
```

![](./imgs/build_timeout.png)

![](./imgs/build_success.png)

![](./imgs/xxx.png)

---
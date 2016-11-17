---
title: Docker初体验
slug: docker-on-windows-experience
id: 64
categories:
  - 技术
date: 2015-07-08T8:26:07+08:00
tags:
  - Docker
---

主要介绍windows平台中的一些体验
<!--more-->

## 安装Boot2docker

从[http://boot2docker.io/](http://boot2docker.io/)可以看到这个软件是为了支持windows和mac的一个中间件包含了一个定制的轻量级VirtualBox虚拟机，最初Docker运行在Linux的哦。
建议完整安装，这样会包括全部所需的内容。

## 镜像

### 获取

    docker pull ubuntu  #下载最新的ubuntu镜像到本地
    docker run -t -i ubuntu //bin/bash  #用ubuntu镜像创建一个容器，在其中运行bash应用，这里的“//”是在windows下面使用的转义符，用“/”会报错。

### 查看镜像信息

    docker images#查看镜像信息
    docker tag#给镜像添加标签
    docker inspect [ID]#获取镜像详细信息

### 搜索镜像

    docker search [term]    #搜索镜像，term参数：
    --automated=false仅显示自动创建的对象 
    --no-trunc=false输出信息不截断显示 
    -s,--stars=0指定仅显示评价为指定星级以上的镜像

### 删除镜像

    docker rmi [image tag or ID]    #删除镜像，强制删除添加-f参数
    docker rm [ID]#删除容器
不建议强制删除，正确的做法是先删除依赖该镜像的容器，再删除镜像

### 创建镜像

    docker commit [opt] CONTAINER [repo[:tag]]#
    -a,--author=""作者信息，
    -m,--message=""提交消息，
    -p,--pause=true提交时暂停容器运行
例如，首先启动一个镜像容器并且在其中创建一个文件，

    docker run -t -i ubuntu //bin/bash

    root@id$touch test

    root@id$exit

然后提交

    $docker commit -m "add a test" -a "mzvast" id test

然后查看本地镜像列表

    $docker images
可以看到多了个test的repo哦！

此外也可以从本地导入镜像比如

    $cat xxx.tar.gz|docker import -ubuntu:14.04，
然后也可以在本地镜像列表中看到

### 导出和载入镜像

    $docker save -o file_name.tar ubuntu:14.04
这样就可以把镜像列表中的ubuntu的tag为14.04的版本导出成tar文件啦

    $docker load --input file_name.tar或者docker load <file_name.tar
这样就可以导入啦

### 上传镜像

    $docker push NAME[:tag]  
这样就可以上传自制镜像到DockerHub网站啦，不过要先注册哦

## 容器

### 创建容器

    docker create -it ubuntu:latest
然后可以

    docker ps -a        #查看所有容器状态
看到创建的容器处于停止状态，可以使用

    docker start    #启动容器

理论上

    docker run = docker create + docker start

    -t选项分配一个伪终端pseudo-tty并且绑定到容器标准输入上，
    -i则让容器的标准输入保持打开状态
    ctrl+d或者exit退出容器

守护态运行通过-d参数实现

    docker run -d ubuntu //bin/sh -c "while true; do echo hello world; sleep 1;done"
容器启动后会返回一个唯一的ID

    docker ps   #查看容器信息
    docker ps logs [id] #查看输出信息

### 终止容器

    docker stop [-t|--time[=10]]    #首先向容器发送SIGTERM信号，等待（默认10秒）一段时间后再发送SIGKILL信号终止容器。
    docker kill [id]    #强行终止容器
    docker ps -aq   #查看处于终止状态的容器
    docker start [id]   #启动终止态的容器
    docker restart [id] #将正在运行的容器重启

### 进入容器

<p>在使用-d参数容器会进入守护态，此时用户无法看到容器的信息，如果要进入容器，有多种方法。

    docker attach [id/name] #缺点是多窗口attach到同一个容器的时候会同步显示，阻塞。
    docker exec ……例如docker exec -ti id //bin/bash，这样就可在容器内开多个bash了
    nsenter工具(enter into namespaces),不推荐，exec是更好的工具。

先体验到这里，后续的内容下次更新咯
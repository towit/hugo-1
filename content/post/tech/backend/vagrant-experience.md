---
title: Vagrant初体验
slug: vagrant-experience
id: 60
categories:
  - 技术
date: 2015-07-07T13:26:00+08:00
tags:
  - vagrant
type: page
---

首先需要下载[vagrantup](https://www.vagrantup.com/)和[virtualbox](https://www.virtualbox.org/)
<!--more-->
## Vagrant常用命令

    vagrant box add {title} {url} #添加一个虚拟机,{url}可以是网址，也可以指向本地的文件,{title}是虚拟机的名称可以自定
    vagrant init {title}  #初始化该虚拟机
    vagrant up    #启动虚拟机
    vagrant halt [name]   #关闭虚拟机
    vagrant box list  #列出当前导入的box
    vagrant destory   #移除虚拟机
    vagrant box remove [name] #移除box
    vagrant status [name] #查看虚拟机的状态
    vagrant ssh   #ssh连接虚拟机

## BOX的来源

可以自己打包box，也可以用其他人打包好的box哦，推荐一个觉得还不错的box站点[vagrantbox.es](vagrantbox.es)

## 关于配置文件

配置文件在当前目录的Vagrantfile文件中

## 体验过程

说道体验我们就来从头到尾走一个过程吧！安装完上面提到的两个软件，在这里面我们以Devopera CentOS 6 LAMP stack这个box为例。Let's Go!

    mkdir vagrant && cd vagrant
   添加box：

    vagrant box add doco6-lamp-dev http://devopera.com/node/63/download/centos6
然后就会自动的用curl下载这个文件了，你也可以先下载好然后给它传入一个本地地址哈。比如这样

    vagrant box add doco6-dj1off-dev \\E:\\doco6-dj1offr006-dev.box     #添加box
    vagrant init doco6-lamp-dev #初始化
这个时候目录下就会出现一个.vagrant目录和一个Vagrantfile文件

    vagrant up #启动虚拟机
如果没有报错，那么就可以通过ssh登录虚拟机了，有点激动哈！

    vagrant ssh #ssh登录虚拟机。成功登录会显示当前虚拟机的配置信息和监听的端口号。
然后就可以运行各种命令了，玩的差不多了，咱们关机吧

    vagrant halt
如果你不再使用这个虚拟机了，想要销毁使用痕迹，运行。这样下次vagrant up的时候又会重新初始化这台虚拟机咯。

    vagrant destroy 
如果你想打包这个虚拟机，那么就要用到打包命令

    vagrant package

可附带参数（原文引用）

    --base NAME - Instead of packaging a VirtualBox machine that Vagrant manages, this will package a VirtualBox machine that VirtualBox manages. NAME should be the name or UUID of the machine from the VirtualBox GUI.
    --output NAME - The resulting package will be saved as NAME. By default, it will be saved as package.box.
    --include x,y,z - Additional files will be packaged with the box. These can be used by a packaged Vagrantfile (documented below) to perform additional tasks.
    --vagrantfile FILE - Packages a Vagrantfile with the box, that is loaded as part of the Vagrantfile load order when the resulting box is used.

那么今天初步的体验先到这里，更多体验期待就下次分享啦~
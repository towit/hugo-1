---
title: CentOS6 VPS安装LNMP+WordPress ip建站全过程
slug: centos6-vps-wordpress
tags:
  - CentOS
  - FTP
  - LNMP
  - VPS
  - WordPress
id: 23
categories:
  - 技术
date: 2015-07-04T19:56:52+08:00
---
搭建环境：搬瓦工512M，5G。
<!--more-->

主要步骤：

1.  安装LNMP
2.  创建数据库
3.  添加虚拟主机
4.  安装WordPress
5.  配置和权限

以下是具体过程

## 安装LNMP和FTP

LNMP（linux+Nginx+MySQL+PHP），一键安装包[http://lnmp.org/install.html](http://lnmp.org/install.html "LNMP一键安装")按照该教程指导进行安装。

首先，推荐使用Xshell作为终端连接VPS（考虑到putty每次登陆都要输入密码，非常的麻烦）

登陆后运行：

    screen -S lnmp #创建一个不会被打断的“后台”

screen常用命令有：

    screen -S name  #创建名为name的屏幕
    (ctrl+a)+d  #暂时中断会话
    screen -ls  #显示会话和ID
    screen -r num   #继续ID为num的会话
安装LNMP执行：

    wget -c http://soft.vpser.net/lnmp/lnmp1.2-full.tar.gz
    tar zxf lnmp1.2-full.tar.gz
    cd lnmp1.2-full
    ./install.sh lnmp
期间需要设置：

*   MySQL密码（回车默认为root）
*   是否启用MySQL InnoDB（n回车）
*   MySQL版本号（默认回车）
*   PHP版本号（默认回车）
*   内存优化其（默认回车不安装）
*   提示"Press any key to install...or Press Ctrl+c to cancel"后，按（回车开始安装）。

等待大约30分钟……安装成功

然后执行安装FTP：

    ./pureftpd.sh 


## 创建数据库

浏览器访问`[你的ip]/phpmyadmin`进入数据库管理后台，新建数据库wordpress

## 添加虚拟主机

运行：

    lnmp vhost add

*   域名设置：这里暂时用[你的ip]，
*   是否还要添加域名：no
*   设置网站目录:回车
*   是否开启伪静态：y回车
*   伪静态类型：wordpress回车
*   是否开启log功能：y回车
*   回车开始安装

因为添加vhost导致“路由”的改变，此时已经不能通过`[你的ip]/phpmyadmin`访问数据库后台了。所以要把phpmyAdmin和pureFTPd、探针等文件复制到`/home/wwwroot/[你的ip]`下面，为了安全记得把phpmyAdmin改一个名字

## 安装WordPress

PS:当前在~路径下操作

   运行命令：

    wget -c https://cn.wordpress.org/wordpress-4.2.2-zh_CN.tar.gz
    tar zxvf wordpress-4.2.2-zh_CN.tar.gz

   权限设置：

    chown -R www:www wordpress

   复制到nginx目录：

    cp -R wordpress/* /home/wwwroot/[你的ip]

（PS：默认情况nginx根目录是：/home/wwwroot/default/，添加vhost以后已经将你的ip绑定到/home/wwwroot/[你的ip]下面）

访问[你的ip]按照向导安装WordPress即可

## 配置和权限

不进行这一步的话在插件安装和更新上会遇到问题。

> 登陆phpMyAdmin如果提示无法读取目录、超时或报502 Bad Gateway错误。需要修改/home/wwwroot/[你的ip]/.user.in将里面的内容删除，重启ftp服务生效。

由于.user.ini文件无法直接修改（设置了特殊属性，查看属性`lsattr .user.ini`）

修改前需要执行：

    chattr -i .user.ini

修改完成后再执行：

    chattr +i .user.ini

> 插件升级提示"要执行请求的操作，WordPress需要访问您网页服务器的权限。请输入您的FTP登陆凭据以继续"并且报错比如无法读取wp-content等等，那么需要执行权限设置操作。

在wp-config.php文件中添加以下内容

    define("FS_METHOD","direct");
    define("FS_CHMOD_DIR", 0777);
    define("FS_CHMOD_FILE", 0777);

## 鸣谢

http://jingyan.baidu.com/article/4f34706efc1237e387b56da4.html

http://www.mengjx.com/setup-lnmp-environment-and-install-wordpress-in-centos.html

http://blog.csdn.net/fox64194167/article/details/33333301
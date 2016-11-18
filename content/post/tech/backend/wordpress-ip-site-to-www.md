---
title: WordPress ip站点迁移到域名站点过程
slug: wordpress-ip-site-to-www
tags:
  - CentOS
  - FTP
  - LNMP
  - VPS
  - WordPress
id: 56
categories: 
  - tech
date: 2015-07-05T15:46:29+08:00
type: page
---
注册域名以及绑定DNS记录的体验。
<!--more-->
## tk顶级域名免费注册

节省篇幅，此处[参考](http://jingyan.baidu.com/article/148a192179dc824d71c3b11b.html "百度经验")

注册有延时，邮件没收到要耐心等一会哈。记得做好dns解析到你的ip哦~

## ip站点迁移到域名

这里用到的还是这三个命令，还记得嘛？

    lnmp vhost {add|list|del}



值得注意的是首先登陆WordPress后台，【设置】>【常规】将其中的WordPress地址和站点地址都改成你的www域名。

然后运行

    lnmp vhost del     #删除ip vhost
    lnmp vhost add     #添加站点www域名
-记得要添加两个哦，一个是“www.域名.com”，另一个是“域名.com”哈。依然设置成伪静态和wordpress哦

接下来是目录迁移带权限

    cp -R /home/wwwroot/ip* /home/wwwroot/域名

然后进入`/home/wwwroot/域名`目录，修改那个带“i”标志的.user.ini文件，这个文件是在lnmp vhost add时候系统生成的呢所以每次add都要修改哈。修改的过程复习一下：因为.user.ini文件无法直接修改

修改前需要执行：

    chattr -i .user.ini
修改完成后再执行：

    chattr +i .user.ini

登陆看看ftp是否正常了呢，测试一下安装新插件，如果提示写入读取出错思密达就试试删除  目录下wp-config.php中的

    define("FS_METHOD","direct");
    define("FS_CHMOD_DIR", 0777);
    define("FS_CHMOD_FILE", 0777);
我在删除之后就正常了哦。
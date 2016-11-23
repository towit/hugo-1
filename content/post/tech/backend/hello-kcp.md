+++
tags = [
  "kcp",
  "kcptun",
  "shadowsocks",
  "raspberrypi",
  "systemd"
]
author = ""
draft = false
slug = "hello-kcp"
type = "page"
title = "树莓派遇上kcptun"
comments = true
share = true
date = "2016-11-19T19:10:18+08:00"
menu = ""
categories = ["tech"]
image = ""

+++
电信和校园网出国线路经常会炸，慢的让人抓狂。Kcptun用来做P2P隧道加速效果显著，尤其适合看视频。本文以centos6和rasbian为例做一个过程梳理。
<!--more-->
## 本文涉及内容
- kcptun服务端的搭建
- kcptun客户端在树莓派上的搭建
- kcptun客户端systemd自启脚本的使用

## 环境
- 搬瓦工512M/centos6
- 树莓派3/rasbian-PIXEL

## 服务端
推荐这款[一键安装脚本](http://www.jianshu.com/p/78420fad1481)
```bash
$wget --no-check-certificate https://raw.githubusercontent.com/kuoruan/kcptun_installer/master/kcptun.sh
$chmod +x ./kcptun.sh
$./kcptun.sh
```
需要配置的内容主要是：

- 设置 Kcptun 的服务端端口
- 设置加速的 IP
- 设置需要加速的端口
- 设置 Kcptun 密码

安装之后，Kcptun服务交由 Supervisor 管理

Kcptun 相关命令：

  - 服务启停：`supervisorctl {start|stop|restart|status} kcptun`
  - 如需更新：`./kcptun.sh update`
  - 如需重新配置：`./kcptun.sh reconfig`
  - 卸载：`./kcptun.sh uninstall`

注意！要把屏幕输出的json内容复制一下，后面会用到。

## windows客户端
首先说说windows的gui程序，先下载一个[配置kcptun的工具](https://github.com/dfdragon/kcptun_gclient/releases)，然后下载[kcptun主程序](https://github.com/xtaci/kcptun/releases)，打开工具，将屏幕输出的内容以json导入配置，设置下本地端口，启动即可。

- 特别注意！ipv6地址在写时候要加`[...]`方括号

## 树莓派客户端
从上面的主程序中找到arm_64bit的版本下载，解压到一个目录，假定为kcp。然后新建三个脚本，`start.sh`，`stop.sh`，`restart.sh`。
内容如下：

- start.sh    

    ```bash
    #!/bin/bash
    ./client_linux_arm7 -l :< local port > -r < ip >:< port > -key "bromide-tavern-sewer" -crypt aes -datashard 10 -parityshard 3 -conn 1 -mtu 1350 -sndwnd 1024 -rcvwnd 1024 -dscp 0 -autoexpire 60 -keepalive 10 -sockbuf 4194304 -mode fast > kcptun.log 2>&1 &
    echo "Kcptun started."
    ```
- stop.sh

    ```bash
    #!/bin/bash
    echo "Stopping Kcptun..."
    PID=`ps -ef | grep client_linux_arm7 | grep -v grep | awk '{print $2}'`
    if [[ "" !=  "$PID" ]]; then
      echo "killing $PID"
      kill -9 $PID
    fi
    echo "Kcptun stoped."
    ```
- restart.sh
    ```bash
    #!/bin/bash
    cd /root/kcptun/
    sh client-stop.sh
    echo "Restarting Kcptun..."
    sh client-start.sh
    ```

## systemd自启服务脚本
手动启动服务总不是个好办法，毕竟重启就得手动运行一遍，实在无聊。  

- 新建一个文件/etc/systemd/system/kcptun.service内容如下

    ```shell
    [Unit]
    Description=Kcptun Client Service
    After=network.target
    [Service]
    Type=simple
    User=nobody
    ExecStart=< 上面的start.sh的主体部分(去掉shebang行)粘贴过来即可 >
    [Install]
    WantedBy=default.target
    ```

- 增加可执行权限  
`sudo chmod +x  /etc/systemd/system/kcptun.service`
- 测试运行  
`sudo systemctl start kcptun.service`
- 启用服务  
`sudo systemctl daemon-reload && sudo systemctl enable kcptun.service`

## 修订说明
----------
时间 | 说明  
:----|:--------
2016-11-23|增加systemd自启脚本
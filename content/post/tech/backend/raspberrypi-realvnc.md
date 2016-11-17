+++
menu = ""
title = "raspberrypi realvnc"
slug = "raspberrypi-realvnc"
image = ""
date = "2016-11-16T21:42:46+08:00"
comments = true
share = true
author = ""
draft = false
tags = [
  "raspberrypi",
  "vnc",
]

+++

在树莓派9月更新raspbian代号为PIXEL系统之后，集成了对realvnc的支持，其中有一些坑，值得注意！<!--more-->

## 准备工作
主要的参考就是这篇[官方教程](https://www.raspberrypi.org/documentation/remote-access/vnc/)。

以下指的成功的定义是：

-  开机自动启动vnc服务

官方指导100%成功的场景是这样的：

- 开机默认启动到桌面模式
- 树莓派配置的interface中开启vnc开关

以下情况下100%失败：

- 开机默认启动到文本模式

研究了一会，跳了几次坑之后，终于明白为什么官方教程主推service mode而晦涩的掩盖了virtual mode。

## 探索
  - 在首次安装完realvnc后出现这一行字，也就是说有两种模式，分别对应两个service

    ```
    Installed systemd unit for VNC Server in Service Mode daemon
    Start or stop the service with:
      systemctl (start|stop) vncserver-x11-serviced.service
    Mark or unmark the service to be started at boot time with:
      systemctl (enable|disable) vncserver-x11-serviced.service
    Installed systemd unit for VNC Server in Virtual Mode daemon
    Start or stop the service with:
      systemctl (start|stop) vncserver-virtuald.service
    Mark or unmark the service to be started at boot time with:
      systemctl (enable|disable) vncserver-virtuald.service
    ```

### service模式详解
  - 服务名：vncserver-x11-serviced.service
  - 连接地址：直接就是ip
  - 树莓派的interface中的VNC控制的即该服务的开关
  - 是可以直接使用的（自带授权），用于以service模式启动vncserver，即通过转发到x11桌面服务来连接。
  - 依赖关系：必须启用lightdm服务（启动到桌面模式会自动带起该服务，但启动到CLI模式则不会带起该服务），否则能连接，但是黑屏。
  - 该模式特殊作用：具有在root权限下运行vncserver的能力，realvnc的托盘区图标也会有所不同来指示这种区别：在该模式下托盘区的图标是黑底，非root模式下托盘区的图标是白底。

### virtual模式详解
  - 服务名：vncserver-virtuald.service：
  - 连接地址：ip：<虚拟桌面号>
  - 是不能直接使用的（需要企业授权），
  - 用于以virtual 模式启动vncserver，即通过创建virtual desktop来连接，
  - 具有按需创建的优点。

#### vncserver的本质
  - 可以看到vncserver是软链接到vncserver-virtual的，也就是运行vncserver创建的是virtual desktop

## 在headless模式下的完美方案
说了那么多，探索这些背后关联的目的就是为了在没有授权的情况下，尽可能的实现virtual模式的自动启动。

  - 主要包括以下两点：
    - 上面两个服务都不需要启用。（通过树莓派配置界面中interface的vnc可以直接关闭service模式）
    - 编写systemd服务来保持virtual desktop的开机启动

### 新建vncserver自启服务
有人已经写好了服务[模板](https://raspberrypi.stackexchange.com/questions/39372/etc-init-d-tightvncserver-script-fails-at-boot)，可以直接拿来用。

- 新建文件 /etc/systemd/system/vncserver@.service 内容如下:

    ```
    [Unit]
    Description=Remote desktop service (VNC)
    After=syslog.target network.target

    [Service]
    Type=forking
    User=pi
    PAMName=login
    PIDFile=/home/pi/.vnc/%H:%i.pid
    ExecStartPre=-/usr/bin/vncserver -kill :%i > /dev/null 2>&1
    ExecStart=/usr/bin/vncserver -depth 24 -geometry 1280x800 :%i
    ExecStop=/usr/bin/vncserver -kill :%i

    [Install]
    WantedBy=multi-user.target
    ```


- 添加可执行权限：
<pre>sudo chmod +x  /etc/systemd/system/vncserver@.service</pre>

- 测试：
<pre>sudo systemctl start vncserver@1.service</pre>
- 自启服务：
<pre>sudo systemctl daemon-reload && sudo systemctl enable vncserver@1.service</pre>
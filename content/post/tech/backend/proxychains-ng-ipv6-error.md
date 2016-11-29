+++
author = ""
title = "proxychains-ng ipv6 错误的解决方案"
slug = "proxychains-ng-ipv6-error"
date = "2016-11-29T22:19:14+08:00"
draft = false
comments = true
tags = [
  "proxychains-ng",
  "ipv6"
]
categories = [
  "tech",
]
menu = ""
type = "page"
image = ""
share = true

+++

当用pyenv在代理的情况下安装python的时候可能会报错，说是You must get working getaddrinfo() function. or you can specify "--disable-ipv6". getaddrinfo() 
<!--more-->
## 错误原因
- 推测原因是来自proxychains-ng对ipv6地址的支持有点问题。

## 解决方案
- 手动临时指定http代理服务器为某ipv4地址的服务器，此时就不需要用Proxychains4命令了  
`http_proxy=127.0.0.1:1080 <命令>`
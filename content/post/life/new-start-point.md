+++
categories = [
  "life",
]
comments = true
author = ""
draft = false
title = "iOS写作测试"
slug = "ios-writing-test"
menu = ""
image = "http://ogscovhkh.bkt.clouddn.com/workflow-IMG_0500.png"
share = true
date = "2017-03-15T22:52:02+08:00"
type = "page"
tags = [
  "随笔"
]

+++
结合多种工具实现在iOS写作
<!--more-->
## 意义
之所以需要实现在iOS的写作。说来有好多原因，首先，相对于PC这种重的客户端来说，手机和平板似乎才是随时随地可以获得的，所谓的真正的移动互联网。

## 问题描述
markdown本身十分轻量，语法简练，最大的问题是多媒体的嵌入，这也是老大难了，关键问题是插入图片。我们需要托管图片，同时获取访问的链接。七牛云作为图床的首选，也正是因为其免费的10G存储，对于普通的博客绰绰有余。

## 使用的软件介绍

- Working copy(管理代码)
- Editorial(编辑working copy中的文件)
- Draft(运行javascript生成七牛云token)
- Workflow(上传图片到七牛云并获取链接)
- 以及在背后默默工作的Travis CI

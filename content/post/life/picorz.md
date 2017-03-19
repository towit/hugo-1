+++
author = ""
menu = ""
share = true
comments = true
tags = ["随笔","markdown","electron","windows"]
title = "markdown图床工具Picorz"
draft = false
image = "http://ogscovhkh.bkt.clouddn.com/Picorz-pasteshot--2017-03-19T19:31:06+08:00"
categories = ["tech"]
date = "2017-03-19T19:54:24+02:00"
slug = "markdown-image-tool-picorz"
type = "page"

+++

基于electron开发了这款图床小工具
<!--more-->

## 起因

之所以要搞这个小工具呢，主要就是markdown传图实在是不方便。现在有的相关产品，要么是不够精致，要么是太大而全，要么是Mac平台，总之就是没有我想要的那种。我的标准是什么呢？

- 功能简单，只能传图
- 不依赖于编辑器

## 看东西

凡是先bulabula的一堆再看东西的都是耍流氓。先来几张图，拿出来溜溜。或者直接去[项目地址](https://github.com/mzvast/Picorz)

### 主界面
![](http://ogscovhkh.bkt.clouddn.com/Picorz-clipboard-)
传图片主要有三种方式，橘黄色的三处均是与上传有关的：

- Drag&Drop区域：整个浅黄色区域都可以拖拽进去
- Paste按钮：将剪贴板的截图图片
- OpenFile按钮：直接打开文件选择器选择文件

### 设置界面
![](http://ogscovhkh.bkt.clouddn.com/Picorz-pasteshot--2017-03-19T17:45:51+08:00)
使用之前，先要去Tools->Settings配置一下七牛的账户

## 关于设计

这个Logo你懂的，无可奉告！

这个软件名称么，大部分传图的工具都叫自己是xxxPic，然后我觉得好看的名字都被取完了。就表示跪了，Pic+Orz，这个就这么来了。

这个配色么，源自于这个：09热情-原色•间色•复色-烂漫
![](http://ogscovhkh.bkt.clouddn.com/Picorz-pasteshot--2017-03-19T19:29:15+08:00)

## 关于程序

本软件是基于[Electron](https://electron.atom.io)框架构建的。没有用任何前端JavaScript框架。应用的编译和打包成安装包是基于appveyor的，可以参见项目的appveyor.yml文件。

友友可以去试用一下，有什么问题可以反馈到github上面，<3。
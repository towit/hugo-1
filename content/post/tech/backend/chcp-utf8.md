+++
author = ""
type = "page"
title = "cmder设置当前活动页utf8编码，完美解决进度条乱码"
comments = true
date = "2016-11-29T12:26:23+08:00"
image = ""
slug = "chcp-utf8"
categories = [
  "tech",
]
share = true
menu = ""
draft = false
tags = [
  "cmder",
  "terminal",
  "windows",
  "yarn"
]

+++

众所周知windows的终端有各种各样的问题，其中默认的字符编码在中文系统中是GBK，因此一些进度条会显示不正常。包括yarn，ssh等都会受到影响。
<!--more-->

## 修复方法分析比较

### 网上流传的方法

- 最简单的修复方法就是设置当前活动页为utf8编码，然而windows10下面很buggy，设置chcp后重新打开终端还是GBK编码。也不是完全无效，具体测试得出就是系统目录下面的cmd.exe改动生效了，但是指向cmd的快捷方式依然不生效。因此通过改动源头cmd来改变编码的路子似乎走不通。
- 有些人想出的方法就是去改注册表，亲测windows10下无效，而且直接修改注册表的关键位置有可能会对系统的运行造成潜在影响。

### 终极解决方案

- 因此，最稳妥的方法就是每次启动终端的时候自动切换当前活动页为utf8，而且借助我们的好朋友cmder，实现起来非常简单，只需要在环境变量(cmder右键->Settings->Startup->Environment)，添加一条`chcp utf-8`，保存，重新打开cmder即可。原理上很简单，就是在每次在cmder中打开个新cmd的时候执行一段bat脚本。
+++
categories = [
  "tech",
]
comments = true
author = ""
draft = false
title = "重新审视bash on ubuntu on windows"
slug = "about-wsl"
menu = ""
image = ""
share = true
date = "2016-12-07T22:52:02+08:00"
type = "page"
tags = [
  "WSL",
  "bashOnWindows",
  "ubuntu"
]

+++
最近倒腾bash on windows，记录一些自己的看法。
<!--more-->

本文旧版本已发表在[知乎](https://www.zhihu.com/question/41967910/answer/134773244)

## bash on windows 存在的问题
### 1.windows下的IDE无法整合WSL
我觉得作为开发环境很重要的一点就是win下的IDE们不能“方便”地直接调用WSL下的程序，比如在vscode下面不能调用WSL里的git，而非得装个git-bash。更别说IDE里直接调用WSL里面的python，node，go了。
根据微软[最新build中](https://blogs.msdn.microsoft.com/commandline/)提出的互操作，重点似乎在WSL侧调用windows程序，他们已经把win下的path都append到WSL的path了，但在windows侧则不可见WSL的path，只能通过蹩脚的bash -c 去调用。

在微软自己都没有搞清楚两者互操作关系的前提下，就会有各种类似于这样的问题：用xxx-for-win还是用xxx-on-WSL？
若只在WSL侧装工具链，windows侧的IDE就是残废成只能做编辑器了，你用的win下的工具链怎么测linux下的bug，怎么加断点调试？

### 2.WSL阉割GUI硬伤导致不具备高效的开发环境
不仅互操作不行，在WSL里面开发体验也是很糟糕的。
之前盛传的在windows下用xserver运行Ubuntu桌面程序的视频也是反映了开发者对缺乏GUI支持的诉求。

### 3.WSL定位究竟是开发环境or生产环境？
甚至我觉得微软的表述是自相矛盾的，他说WSL是面向开发，但无论怎么看没有GUI的Ubuntu更像是一个部署环境，而不是开发环境，没有IDE的支持怎么调试怎么开发呢？而且按照现在的完成度，只是个玩具。

### 4.关于WSL未来的几点想法
  - 微软绝对不会放弃windows下工具链的支持
  - WSL短期内不能取代windows下工具链，至少从IDE支持度上差距很大
  - 微软的目前的所有动作看起来都似乎表明WSL的真实目的是让linux开发者用dotnet和SQLserver，也就是把linux的开发者拉过去，而不是把windows的开发者推出去。
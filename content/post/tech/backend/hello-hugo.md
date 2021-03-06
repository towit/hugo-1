+++
comments = true
draft = false
share = true
author = ""
title = "Hugo借助Travis CI实现GitHub Pages自动部署实践"
tags = [
  "hugo",
  "go",
  "travis"
]
categories = ["tech"]
image = ""
menu = ""
date = "2016-11-17T16:12:18+08:00"
slug = "hello-hugo-and-more"
type = "page"

+++
这两天把博客从hexo转移到了hugo，并实现了自动化构建，借此机会趁热打铁把过程记录一下。<!--more-->
## 内容概述
- hugo的安装
- 子模块同步
- 语法高亮
- Travis CI配置

## 环境介绍
- win10x64 一周年纪念版及更新版本
- [chocolatey](https://chocolatey.org)包管理器
- [GitBash](https://git-scm.com/downloads)
- Python 2.7+或3.5+

## Hugo的安装
[hugo](https://gohugo.io/)的安装很简单，管理员权限下输入该命令：
```
choco install -y hugo
```
基本的hugo命令可以参考官网的[快速上手](https://gohugo.io/overview/quickstart/)，详解hugo的话又可以单独开篇文章了，本文的重点是自动化部署，赶紧进入重点。

## 子模块同步
为什么这里要提到子模块呢？主要原因是hugo自己不带主题，主题是单独放入在themes路径下的。
通常比较合适的做法不是去直接clone别人的主题，而是先fork，然后clone自己的这个目录。
下一条添加子模块的命令：
```
git submodule add < github url> themes/casper
```
## 语法高亮
hugo对服务端语法高亮支持的不错，借助于python插件[Pygments](http://pygments.org/)。
只要一条命令：
```
pip install Pygments
```
然后在hugo的主配置文件`config.toml`里面加入相关配置即可，贴上我的供参考：

```
# color-codes for highlighting derived from this style
pygmentsStyle = "xcode"
# true: use pygments-css or false: color-codes directly
pygmentsUseClasses = false
# add syntax highlighting with GitHub flavoured code fences 
PygmentsCodeFences = true
```


## Travis CI的配置
看到网上许多的配置搞的来比较复杂，有的还抄来抄去抄错了。

我做了个直观易懂的配置，基本都是常见命令。云端构建一次平均20多秒。
直接上配置文件`travis.yml`：

- 注意要先生成GH_TOKEN[(官方教程链接)](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)。也就是在下图所示页面生成一个token：![](http://ogscovhkh.bkt.clouddn.com/Picorz-pasteshot--2017-11-14T23:43:35+08:00)
- 然后把这个token添加到[Travis](https://travis-ci.org/)的环境变量里去。如下图所示
![](http://ogscovhkh.bkt.clouddn.com/Picorz-pasteshot--2017-11-14T23:50:39+08:00)
![](http://ogscovhkh.bkt.clouddn.com/Picorz-pasteshot--2017-11-14T23:48:23+08:00)。
- 最后简单介绍构建脚本的编写[(构建的生命周期)](https://docs.travis-ci.com/user/customizing-the-build/#The-Build-Lifecycle)。由于直接用go get < github url >的方式下载到的不是稳定版，为了和本地环境一致，需要用下载编译后可执行文件的方式，并可以指定版本号。因此，并不需要CI安装了go语言。

``` bash
language: go

go:
  - 1.7.3

branches:
  only:
    - source

env:
 global:
   - GH_REF: github.com/mzvast/mzvast.github.io.git

sudo: required

git:
  submodules: false

# Use sed to replace the SSH URL with the public URL, then initialize submodules
before_install:
  - sed -i 's/git@github.com:/https:\/\/github.com\//' .gitmodules
  - git submodule update --init --recursive

install:
  - export HUGO_VERSION=0.30.2
  - sudo pip install Pygments
  - wget https://github.com/spf13/hugo/releases/download/v$HUGO_VERSION/hugo_${HUGO_VERSION}_Linux-64bit.tar.gz
  - tar xzf hugo_${HUGO_VERSION}_Linux-64bit.tar.gz
  - chmod a+x hugo

script:
  - ./hugo

after_script:
  - cd ./public
  - git init
  - git config user.name "mzvast"
  - git config user.email "mzvast@gmail.com"
  - git add .
  - git commit -m "Update docs"
  - git push --force --quiet "https://${GH_TOKEN}@${GH_REF}" master:master
```
## 修订说明
----------
时间 | 说明  
:----|:--------
2017-11-15|增加插图，更新部分说明性内容。
2016-11-19|修改Travis配置文件中hugo的下载方式，确保版本为0.17，同时加快构建速度到20秒。
2017-09-14|更新Travis配置文件中hugo版本。
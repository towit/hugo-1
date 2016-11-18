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
- win10x64 一周年纪念版
- [chocolatey](https://chocolatey.org)包管理器
- [GitBash](https://git-scm.com/downloads)
- Python 2.7+或3.5+

## Hugo的安装
[hugo](https://gohugo.io/)的安装很简单，管理员权限下输入该命令：
```
choco install -y golang hugo
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

- 注意要先生成好GH_TOKEN并加入到Travis的环境变量里去：
- 注意直接用go get < github url >的方式下载到的不是稳定版，为了和本地环境一致（0.17版本），需要用下载编译后可执行文件的方式。

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
  - sudo pip install Pygments
  - wget https://github.com/spf13/hugo/releases/download/v0.17/hugo_0.17_Linux-64bit.tar.gz
  - tar xzf hugo_0.17_Linux-64bit.tar.gz
  - mv hugo_0.17_linux_amd64/hugo_0.17_linux_amd64 hugo
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
2016-11-19|修改Travis配置文件中hugo的下载方式，确保版本为0.17，同时加快构建速度到20秒。
+++
slug = "python-conda"
comments = true
draft = false
tags = [
  "python",
  "conda",
]
share = true
author = ""
type = "page"
categories = [
  "tech",
]
title = "python版本管理之conda"
image = ""
menu = ""
date = "2016-11-29T11:32:25+08:00"

+++
之前提到过windows下没有pyenv这等神器，但是我们有conda，这也是一款功能强大的作品！
<!--more-->

## 适用环境
- 全平台(windows、linux、macOS)
- 本文主要针对windows
- 参考这段[视频](https://www.youtube.com/watch?v=YJC6ldI3hWk)

## 安装conda
- 这里仅安装包管理器[miniconda](http://conda.pydata.org/miniconda.html)，而不是400MB+的conda(包含n多科学计算库)
- miniconda版本的选择并不特别重要，因为conda可以以项目为单位，安装多个python版本，并在之间切换。这里以安装[自带Python 3.5的版本](https://repo.continuum.io/miniconda/Miniconda3-latest-Windows-x86_64.exe)为例

## 基本使用，以创建flask项目为例
- 创建环境，并安装flask包

    ```bash
    #conda create --name <env名称> <要安装的包(可选),空格分隔>  
    conda create --name my_app flask
    ```  

- 激活/退出该环境

    ```bash
    activate my_app
    deactivate my_app
    ```

- 查看所有的环境

    ```bash
    conda env list
    ```

- 删除某环境

    ```bash
    conda remove --name my_app --all
    ```

## 兼容性提示

- 假如当前chcp是utf-8，那么在python2.7下会报错 `LookupError: unknown encoding: cp65001`
- 解决方法是在cmder的环境变量加入一条`set PYTHONIOENCODING=utf-8`
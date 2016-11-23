+++
slug = "python-pyenv"
categories = [
  "tech",
]
menu = ""
date = "2016-11-23T21:21:57+08:00"
tags = [
  "python",
  "pyenv",
  "virtualenv",
  "flask"
]
comments = true
share = true
author = ""
type = "page"
draft = false
image = ""
title = "Python版本管理之pyenv"

+++
python的版本管理一直都挺麻烦的感觉，最近花时间研究了下，发现个pyenv这个神器，相当于node的nvm，用起来非常熟悉。
<!--more-->
## 环境
- 类unix系统
- 不支持windows
## 安装

- 前置需求
  - Ubuntu/Debian:  

        ```bash
        sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev 
        libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev xz-utils
        ```
  - Fedora/CentOS/RHEL:  

        ```bash
        yum install zlib-devel bzip2 bzip2-devel readline-devel sqlite sqlite-devel openssl-devel
        ```
  - Mac OS X:

        ```bash
        brew install readline xz
        ```
- 运行一键安装脚本(安装完自带virtualenv插件)
```bash
$ curl -L https://raw.githubusercontent.com/yyuu/pyenv-installer/master/bin/pyenv-installer | bash
```
- 然后根据提示可能需要把这段加入到~/.bash_profile,重载一下shell
```bash
export PATH="~/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
```

## 以创建flask项目为例测试
- 在任意位置新建flask目录:  
`mkdir -p pythonProject/flask&&cd pythonProject/flask`
- 安装3.5.2版本python:  
`pyenv install 3.5.2`
- 在3.5.2版本下创建名为flask的虚拟环境(这个flask的命名和上面那个目录名没有任何关系):  
`pyenv virtualenv 3.5.2 flask`
- 新建pyenv用于识别环境的文件(pyenv通过该文件识别这个目录的虚拟环境)：  
`echo "flask"> .python-version`  
此时可以看到命令提示符前出现了`(flask)`的标识，说明现在的环境与系统环境隔离了
- 安装Flask包
`pip install Flask`
- 新建flask demo主程序
`cat >app.py`
粘贴这段

    ```python
    from flask import Flask
    app = Flask(__name__)

    @app.route("/")
    def hello():
        return "Hello World!"

    if __name__ == "__main__":
        app.run()
    ```
回车,ctrl+D
- 运行`python app.py`可以看到程序正常运行了

## 后记
这样一来在linux系统上的python版本管理问题是解决了，而且使用起来跟nvm特别接近，但在windows环境并不能用这个程序，有待进一步探索。
## 参考
- [pyenv](https://github.com/yyuu/pyenv)  
- [pyenv-installer安装器](https://github.com/yyuu/pyenv-installer)  
- [pyenv-virtualenv插件](https://github.com/yyuu/pyenv-virtualenv)  
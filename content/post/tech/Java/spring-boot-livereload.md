+++
image = ""
date = "2017-03-22T10:54:24+02:00"
type = "page"
author = ""
tags = ["java","spring","intellij"]
draft = false
title = "spring-boot开启livereload"
menu = ""
share = true
comments = true
categories = ["tech"]
slug = "spring-boot-livereload"

+++
关于开启spring boot devtools的livereload的功能。
<!--more-->

## 目的

之所以要livereload是因为之前习惯了JavaScript开发的过程中各种livereload，所以不能自动刷新简直不能忍吖！这样一来就可以集中注意力在代码上，提高效率。



## 实现步骤

这边以idea为例，主要有三个地方要注意。

1. 首先以maven为例，在maven的配置文件里面加入插件

    ![](http://ogscovhkh.bkt.clouddn.com/Picorz-pasteshot--2017-03-22T20:20:00+08:00)
    
    ```xml
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <optional>true</optional>
        </dependency>
    </dependencies>
    ```

2. 在idea的设置中开启自动编译(Compiler->Build project automatically)

    ![](http://ogscovhkh.bkt.clouddn.com/Picorz-pasteshot--2017-03-22T20:11:55+08:00)

3. 在idea的registry里面开启允许运行时自动编译

    用快捷键`Shift+Ctrl+P`调出搜索框，键入`registry`并回车
    ![](http://ogscovhkh.bkt.clouddn.com/Picorz-pasteshot--2017-03-22T20:13:20+08:00)

    然后滚动到这个`compiler.automake.allow.when.app.running`，打勾保存。
    ![](http://ogscovhkh.bkt.clouddn.com/Picorz-pasteshot--2017-03-22T20:14:49+08:00)

## 参考

- [https://patrickgrimard.io/2016/01/18/spring-boot-devtools-first-look/](https://patrickgrimard.io/2016/01/18/spring-boot-devtools-first-look/)
- [https://spring.io/guides/gs/serving-web-content/](https://spring.io/guides/gs/serving-web-content/)
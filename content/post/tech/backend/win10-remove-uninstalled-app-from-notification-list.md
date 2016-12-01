+++
title = "win10 移除通知中心列表中已卸载的app图标"
comments = true
share = true
slug = "win10-remove-uninstalled-app-from-notification-list"
tags = [
  "win10",
  "windows",
]
categories = [
  "tech",
]
image = ""
menu = ""
author = ""
date = "2016-12-01T23:17:00+08:00"
draft = false
type = "page"

+++
windows10的通知中心中的app列表会随着时间的推移而越来越长，windows并不会自动去检测这些项目是否已经卸载了。有些黑漆漆的图标看着真的很不爽，尤其是具有强迫症的人，更是如此，今天就让我研究了半天，终于找到的解决方案。
<!--more-->

## 环境
- windows 10 1607
- 具有sqlite数据库编辑功能的软件，如navicat

## 原理
微软把这些推送通知相关的数据存储在了一个SQLite数据库中，由于程序卸载后没有调用hook，导致项目残留。

## 数据库位置
- `%localappdata%\Microsoft\Windows\Notifications\wpndatabase.db`

## 执行SQL语句批量处理废弃应用
- 主要处理的是非UWP应用，根据特征（PrimaryId中带有explorer.notification）可以识别。批量删除，无需重启，立即生效。

    ```sql
    delete from NotificationHandler where HandlerType = 'app:desktop' and  PrimaryId like '%explorer.notification%'
    ```
问题到此已经完美解决了。

## 后记
由于一开始以为微软是写在注册表里面的，所以找来找去只找到了一串`explorer.notification{xxx}`形式的键名，所在键位为`HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Notifications\Settings\`
，以及发现了图标缓存的位置为`C:\Users\mzvast\AppData\Local\Microsoft\Windows\Explorer\NotifyIcon`。

之前也Google过中文和英文的资料，都没有人提到重点，后来无意间看到一篇德文的帖子，然后顺藤摸瓜找到了一个很详尽的讨论帖。但是中文方面，始终没有一个明晰的教程。我就记录一下，希望可以帮助到强迫症们！

## 参考链接
- [德文原帖](https://www.deskmodder.de/wiki/index.php?title=Benachrichtigungen_und_Aktionen_Alte_Eintr%C3%A4ge_entfernen_l%C3%B6schen_Windows_10)
- [英文院帖](https://answers.microsoft.com/en-us/windows/forum/windows_10-other_settings/remove-an-uninstalled-app-from-the-notification/6cb57f86-23fd-4261-8922-e49f344095e5?page=9)
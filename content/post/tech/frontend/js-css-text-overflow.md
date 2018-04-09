+++
date = 2018-04-09T12:49:19+08:00
draft = false
title = "关于文本缩略"
slug = "js-css-text-overflow"
tags = ["css","js"]
categories = ["tech"]
image = ""
comments = true	# set false to hide Disqus
share = true	# set false to hide share buttons
menu= ""		# set "main" to add this content to the main menu
author = ""
type = "page"
+++

有时候需要对超过尺寸的文本进行缩略，JS 和 CSS 各自的缩略方法有哪些？

<!--more-->

## 需求描述

*   一个不定宽度的 div，其中的文本当超出 div 宽度时候自动缩略成...
*   中英文混排

## 纯 CSS 的方案（推荐）

```css
div{
    overflow: 'hidden',
    whiteSpace: 'nowrap',
    textOverflow: 'ellipsis'
}
```

textOverflow 这个属性，有“clip|ellipsis|string”三种属性，作用分别如下

| 值       | 描述                                 |
| :------- | :----------------------------------- |
| clip     | 修剪文本。                           |
| ellipsis | 显示省略符号来代表被修剪的文本。     |
| string   | 使用给定的字符串来代表被修剪的文本。 |

## 纯 JS 的方案（不推荐）

JS 在实现这块就显得比较不便，首先，仅仅根据字符数量进行截取，不能很好适应动态的 div 宽度。这里仅给出固定宽度（rem 定义 div 宽度）下可能的办法（但是没法保证不同浏览器和不同默认字体下的一致性）。下面这个 sub 函数就是截取固定长度（先把中文统一转成英文字符计算宽度）。

```javascript
String.prototype.len = function() {
    return this.replace(/[^\x00-\xff]/g, 'rr').length;
};
String.prototype.sub = function(n) {
    let r = /[^\x00-\xff]/g;
    if (this.replace(r, 'mm').length <= n) {
        return this;
    }
    // n = n - 3;
    let m = Math.floor(n / 2);
    for (let i = m; i < this.length; i++) {
        if (this.substr(0, i).replace(r, 'mm').length >= n) {
            return this.substr(0, i) + '...';
        }
    }
    return this;
};
```

---
layout: post
title: "svn update Cannot rename svn/entries 错误"
date: 2012-01-11 20:34
comments: true
categories: Management
---
今天mac下svn update遇到这个奇怪问题，解决方法很简单，在svn目录下运行命令（注意最后是 **有.符号 **）
```
    chflags -R nouchg .
```

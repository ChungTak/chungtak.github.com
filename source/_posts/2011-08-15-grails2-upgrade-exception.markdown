---
layout: post
title: "Grails2 upgrade exception"
date: 2011-08-15 12:15
comments: true
categories: Grails
---
Grails1.x升级到Grails2.x又遇到一个异常
```
Error configuring application listener of class org.codehaus.groovy.grails.web.util.Log4jConfigListener**
Message: org.codehaus.groovy.grails.web.util.Log4jConfigListener
```
这个很容易解决，很多人认为是Log4j问题，其实不是。
之前的项目运行过 *grails install-templates* 命令，在生成的web.xml下添加了内容。
新版本的grails修改过默认web.xml的内容，因此与现在的web.xml冲突。
解决办法是先把目前的web.xml文件备份并删除 ，运行一次 *grails install-templates* 命令生成新的web.xml
再把新增的内容手动添加上去

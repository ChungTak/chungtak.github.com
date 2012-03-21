---
layout: post
title: "Mac 连接不了samba"
date: 2011-05-07 12:13
comments: true
categories: Mac
---
Mac连接samba或者windows共享，一般方法直接在Finder --前往--连接服务器。中输入”smb://ServerIpAddress“ &nbsp;就可以了。
今天连接一个新建的samba服务器，怎么连接也提示密码错误！
原来是由于Samba 配置文件里面的 “netbios name” 名称太长，引起的。
详细参考[连接](http://blog.sonitech.org/2011/05/04/%E8%A7%A3%E5%86%B3-mac-%E8%BF%9E%E6%8E%A5-samba3-%E7%9A%84%E9%97%AE%E9%A2%98/)



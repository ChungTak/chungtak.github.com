---
layout: post
title: "解决Windows与Mac时间不同步"
date: 2012-10-10 09:48
comments: true
categories: Mac 
---

安装Windows 和 Mac 苹果双系统后，系统时间出现不同步。有两种简单的解决办法： 
 
* 方法一：在Mac下`系统编好设置` －> `时间与日期` -> `时区` 最接近的城市选择为`雷克雅未克 - 冰岛`  


![Smaller icon](http://ww3.sinaimg.cn/large/a74ecc4cjw1dxpvrxc7f8j.jpg "")

* 方法二：修改windows注册表，在`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\TimeZoneInformation\`中加一项DWORD，`RealTimeIsUniversal`，把值设为 **1**。


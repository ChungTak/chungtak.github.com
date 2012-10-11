---
layout: post
title: "Java  webservice  jax-rs 中文乱码"
date: 2011-03-02 12:08
comments: true
categories:
 - JAVA
---

今天遇到个奇怪问题，系统用webservice 接收请求，然后调用电信的宽乐网关接口发送短信。
系统编码已经统一为UTF-8，服务器为Ubuntu。系统日志编码正常，但手机接收到的中文短信为乱码。
JAVA_OPTS设置为-Dfile.encoding=UTF8和-Dfile.encoding=GBK还是不行。
最后设置为 **-Dfile.encoding=EUC-CN** 竟然奇迹地成功了 ，运行一切正常。

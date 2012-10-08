---
layout: post
title: "Twitter Bootstrap Modal插件点击背景自动消失"
date: 2011-11-14 10:31
comments: true
categories: CSS
---
我是在Twitter Bootstrap2.0.1中遇到这个问题。
问题描述：
直接调用
``` javascript
$('#myModal').modal('show')；
```
显示的Modal存在一个问题，就是当你鼠标点击Modal后面的背景，Modal会自动隐藏消失！！！
只要添加一个参数就可以解决这个问题
``` javascript
$('#myModal').modal(backdrop:'static');
```
官方文档显示backdrop的类型是boolean，实际上设置true和false都不行，文档上有错误应该为String类型才对

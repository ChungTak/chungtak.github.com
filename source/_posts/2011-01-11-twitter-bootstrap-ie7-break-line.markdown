---
layout: post
title: "twitter bootstrap IE7下断行问题"
date: 2011-01-11 04:04
comments: true
categories: CSS
---
twitter bootstrap2.0有一个bug，在html上添加注释会引起断行（IE7下，IE8，9没问题）
例如以下官方代码:
```
<div class="container-fluid">
    <div class="row-fluid">
        <div class="span4">first</div>
        <div class="span8">second</div>
    </div>
</div>
```
效果为显示一行，左侧显示first字符，右侧显示second字符，非常简单。但如果添加注释，就发生问题了。
```
<div class="container-fluid">
    <div class="row-fluid"><!--顶部logo-->
        <div class="span4">first</div>
        <div class="span8">second</div>
    </div>
</div>
```
在row-fluid后添加注释&lt;!--顶部logo--&gt;,在IE7下运行显示first和second各自一行了！
以上测试环境在IETester上测试，没在纯IE7下使用过。
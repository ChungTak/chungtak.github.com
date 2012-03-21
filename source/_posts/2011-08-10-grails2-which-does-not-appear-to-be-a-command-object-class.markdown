---
layout: post
title: "Grails2升级异常which does not appear to be a command object class."
date: 2011-08-10 12:15
comments: true
categories:
 - Grails
 - Groovy
---
今天把老项目Grails1.1升级到Grails2.01，出现奇怪的错误
```
     XXXController.groovy: The [someAction] action accepts a parameter of type [EventTypeEnum]
     which does not appear to be a command object class. This can happen if the source code for this  class is not in
     this project and the class is not marked with @Validateable.
```
查了一下文档，原来grails2.x把Controller里面的所有public方法都会发布为action。
grails1.x Controller的action写法必须为
```
def exportExcelSingle = {
    ...
}
```
而grails2.x公共方法也会发布为actiong，上面写法等同于
```
public void exportExcelSingle = {
    ...
}
```
修复方法很简单，把public或者def开头的这些私有方法，声明为 **private** 就ok
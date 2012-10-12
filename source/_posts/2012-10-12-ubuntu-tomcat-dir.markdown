---
layout: post
title: "ubuntu tomcat 目录"
date: 2012-10-12 09:13
comments: true
categories: Linux 
---

在ubuntu系统上使用apt-get安装tomcat的目录结构：
 
+ /etc/tomcat7 - 全局配置

+ /usr/share/tomcat7/ - 程序主目录

+ /usr/share/tomcat7/conf/Catalina/localhost/ - 本机部署的 Catalina 配置

  
+ /var/lib/tomcat7/ - 工作主目录

+ /var/lib/tomcat7/webapps - （应用文件war包放在这里）

+ /var/lib/tomcat7/work
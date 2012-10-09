---
layout: post
title: "octopress 常见问题"
date: 2012-10-09 14:12
comments: true
categories: Web
---


+ **Mountain Lion 编译报错extconf.rb:21:in `<main>': Only Darwin systems**

Mountain Lion执行bundle install的时候，会给出下面的报错:

``` tex
creating Makefile
extconf.rb:21:in `<main>': Only Darwin systems
greater than 8 (Mac OS X 10.5+) are supported (RuntimeError)
```

这个报错的原因是octopress需要的`rb-fsevent`的版本是`0.4.3.1`，这个版本还不支持`Mountain Lion`。
简单的处理办法是修改~/.rvm/gems/ruby-1.9.3-p194/gems/rb-fsevent-0.4.3.1/ext/extconf.rb文件，找到19行，如下:
``` bash
sdk_version = { 9 => '10.5', 10 => '10.6', 11 => '10.7' }[darwin_version]
```
修改为
``` bash
sdk_version = { 9 => '10.5', 10 => '10.6', 11 => '10.7',12=>'10.8' }[darwin_version]
```

删除第24行内容，如下
``` bash
-isysroot #{xcode_path}/SDKs/MacOSX#{sdk_version}.sdk
```

然后执行`bundle update`编译安装即可。
<!--more-->

+ **rake deploy不能发布，但是预览正常。检查github上source分支代码已更新，但master仍为老代码。**

rake操作应该在source分支下进行，若是刚从github里clone下来的，请先执行`$ git checkout source `

发现是因为代码是新从github下clone下来的，未进行初始化deploy.需要执行`$ rake setup_github_pages`进行初始化。

+ **fatal: 'octopress' does not appear to be a git repository**
命令行执行：`$ git remote add octopress https://github.com/imathis/octopress.git`

+ **undefined method \`[]\' for nil:NilClass**

执行`$ rake setup_github_pages`初始化出现以下异常

``` tex
rake aborted
!undefined method `[]' for nil:NilClass
```

原因是url写错，正确格式为`git@github.com:your_username/your_username.github.com`
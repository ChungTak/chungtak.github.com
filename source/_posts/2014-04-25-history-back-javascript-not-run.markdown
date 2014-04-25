---
layout: post
title: "浏览器后退js不执行"
date: 2014-04-25 22:41:34 +0800
comments: true
categories: javascript
---

当点击后退按钮返回之前访问过的页面时,该网页上的脚本将无法再运行。

解决办法 设置一个空的window.onunload函数

e.g.

``` html Javascript
<html><body>
<script type="text/javascript">
  window.onload = function() { alert('页面加载运行.'); };
  window.onunload = function(){};
</script>
</body></html>
```
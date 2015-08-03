title: "开发WordPress插件的时候，用date函数无法获取所在时区的时间"
date: 2014-07-31 14:59:11
tags:
- php
- wordpress
categories:
- PHP
---
在普通PHP开发中，假定在php.ini中已经设定了timezone为`Asia/Shanghai`之后，用`date('H:i:s')`即可获得当前时间。
但是刚刚在研究开发WordPress插件的时候，发现用`date('H:i:s')`获取的时间是0时区，php.ini中的设置没有起作用，<!-- more -->WordPress后台设置的时区也不在此起作用。
Google了许久也没有找到问题，还去stackoverflow上开了“处女“问。只能暂时搁置了。
##找到答案
吃完午饭又开始折腾，突然脑洞大开，会不会是搜索关键词不好？于是又换了几个关键词Google了下，果然找到了答案：在WP中，需要用其自己的函数`current_time("H:i:s")`才能获取到本时区时间，用`date('H:i:s')`只能得到0时区的时间。
文档在这里：[Function Reference:current_time](http://codex.wordpress.org/Function_Reference/current_time)

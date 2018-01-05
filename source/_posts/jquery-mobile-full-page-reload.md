---
title: 'jQuery Mobile页面刷新JS失效问题'
date: 2014-06-24 16:35:42
tags:
- jQuery Mobile
- js
categories:
- js
---
用jQuery Mobile做了几个简单的演示页面，单个页面开发的时候都很顺利，最后汇总在一个测试的时候，发现切换页面的时候，CSS和JS会失效，一开始一直都以为是自己js代码哪里写错了，导致不能执行。<!-- more -->

翻来覆去得调试，都没有发现问题。最后回去看了下jQurey Mobile的文档，发现这是一项机制：
`"It's important to note that if you are linking from a mobile page that was loaded via Ajax to a page that contains multiple internal pages, you need to add a rel="external" or data-ajax="false" to the link. This tells the framework to do a full page reload to clear out the Ajax hash in the URL. This is critical because Ajax pages use the hash (#) to track the Ajax history, while multiple internal pages use the hash to indicate internal pages so there will be conflicts in the hash between these two modes."`

例如：`<a href="multipage.html" rel="external">Multi-page link</a>`

哎，看文档不仔细啊～

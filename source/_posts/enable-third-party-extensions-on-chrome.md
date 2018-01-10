---
title: Chrome中启用第三方扩展
date: 2014-06-10 17:52:12
tags:
  - chrome
categories:
  - 其他
---
Google发布了Chrome扩展的最新政策，不是Chrome商店的扩展一律停用。
乍一看，出路只有两条了，一是将扩展发布到Chrome应用商店；二是将扩展解压至一个文件夹，然后以“开发者模式”直接导入。
但是类似**Youtube Center**之类的插件是肯定没有机会进Chrome应用商店的，谁会把房子租给一个小偷，方便他来偷自家东西？

## "Challenge except"

感谢的userscripts.org社区，催生了<!-- more -->**[Tampermonkey](https://chrome.google.com/webstore/detail/tampermonkey/dhdgffkkebhmkfjojejmpbldmpobfkfo)**这样一款伟大的插件，关键居然还通过了Chrome应用商店的审核！

## 进入主题

到Chrome应用商店安装**Tampermonkey**，默认配置情况下Tampermonkey会自动嗅探插件脚本，比如Youtube Center在Github上的地址是`https://github.com/YePpHa/YouTubeCenter/raw/master/dist/YouTubeCenter.user.js`，来到这个页面后，Tampermonkey会提示自动添加这个脚本。当然也可以手动添加。
于是，被Chrome禁用的扩展，又复活了。
![Install Youtube Center on Tampermonkey](/public/tampermonkey.png)

## 担心

万一哪天Google看Tampermonkey不爽，把它下架了，那就悲剧了～

---
title: brew upgrade 升级把我的node环境给搞坏了
date: 2018-06-07 15:33:21
tags:
  - node
  - nvm
  - homebrew
categories:
  - 前端
---
起因：sourcetree 提示升级，点了 update，可惜公司网络太挫升级不了。脑抽想到brew好久没更新了，于是`brew update`，哦哟！确实好多可以升级的。随即开上代理并`brew upgrade`，一阵升级好痛快！

升级完毕，愉快得打开vscode并`yarn dev`，报错了...

vscode 终端每次加载都会提示`nvm is not compatible with the npm config "prefix" option: currently set to "/usr/local"`

<!-- more -->

`nvm ls`指向`system`即最新稳定版10.4.0（brew刚升级的）`nvm use 8.11.2`，重启vscode，还是指向`system`

排查，各种google、stackoverflow查，各种改`.bash_profile`、`.zshrc`配置，还是没用。

后来一想，一定是brew导致的！

> 修复过程
- brew 卸载新版 yarn、node
- 重新无依赖安装yarn `brew install yarn --without-node`
- nvm重新指向 `nvm use 8.11.2`
- 重启vscode

>扩展阅读
`brew update` 屏蔽指定软件 `brew pin <formula>`
取消屏蔽 `brew unpin <formula>`

---
title: 使用项目本地gulp代替全局的
date: 2018-06-24 11:08:28
tags:
  - npm
  - gulp
categories:
  - 前端
---

Jenkins上全局安装了`3.9.1`版本的`gulp/gulp-cli`，但是其他项目已经迁移到了`4.0.0`，于是Jenkins编译直接运行`gulp`命令会调用全局的，本项目就报错编译失败了。

解决办法：编译命令最前加一个`npm link gulp`，指明`gulp`使用本地`node_modules`内的版本。
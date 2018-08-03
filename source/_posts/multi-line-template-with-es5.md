---
title: ES5多行模板
date: 2018-08-03 10:40:54
tags:
  - js
categories:
  - 前端
---

ES6开始有了模板，很方便。即使面对ES5环境，也可以用Babel转换做兼容。

但是有时候一些老项目，没办法大改项目架构，只能手动支持ES5。

废话少说，show me the code：

```js
var template = (function () {/*
  <div>
    <input type="text" name="username" value="{{username}}">
  </div>
*/}).toString().match(/[^]*\/\*([^]*)\*\/\}$/)[1];

// 结合模板引擎使用
Mustache.parse(template);
$('body').append(Mustache.render(_template, {username: 'Jack'}));
```
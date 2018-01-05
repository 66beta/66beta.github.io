---
title: '为Bootstrap忽略sublimelinter htmltidy的空标签提示'
date: 2015-08-03 10:46:05
tags:
- sublime
categories:
- buzz
---
找到Sublimelinter的对应配置（Windows），`Preferences > Package Settings > SublimeLinter > Settings User`，针对htmltidy添加设置：

```json
"htmltidy": {

    "ignore_match": ["trimming empty <span>"]

}
```

添加以上设置后，用Bootstrap的时候就不会提示空`<span>`了

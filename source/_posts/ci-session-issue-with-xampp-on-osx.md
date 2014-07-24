title: 'CI Session regenerate Issue with xampp on OSX'
date: 2014-07-21 13:28:21
tags:
- Codeigniter
categories:
- PHP
---
最近赶一个项目，前后端都包了，还久没写PHP了，再一次扮演全栈码农。由于项目冲突了，时间很急，所以加班到半夜，回去还继续写。由于家里是macbook，平时有用xampp，但是一直都没有涉及到Session，所以都没有碰到此问题。Session的设置参考的官网文档的，存在db中。<!-- more -->
问题就是每次刷新页面，session就是会新生成一个，所以永远都拿不到之前一步保存的数据。
经过Google再Google之后，碰到一个有类似问题的老外，就照着改了下配置：
`$config['sess_encrypt_cookie'] = FALSE;`
居然OK了～
也不知道是啥原理导致的，记得之前玩Laravel的时候没碰到过这个问题的。
记录下，有空回头看看到底是CI的bug，还是xampp的配置bug。
title: '如何安装配置Hexo'
date: 2014-05-06 16:29:59
tags: 
- hexo
categories:
- nodejs
---
[Hexo](http://hexo.io/) - 是一个基于Nodejs开发的静态blog系统，类似于基于Ruby的Jekyll。由一个台湾同胞[tommy351](http://zespia.tw/)开发的。在[Github](https://github.com/tommy351/hexo)上搜索"node.js blog"的话，当前是Most stars的。

**Hexo的安装也非常便捷(Git等常用工具就不多说了～)：**
##搭建环境
<!-- more -->
1. 安装nodejs，可以从[nodejs官网](http://nodejs.org/)下载到最新稳定版本。
OSX/Linux用户建议直接安装[nvm (Node Version Manager)](https://github.com/creationix/nvm)，然后用nvm安装指定的nodejs版本；
Windows用户没有nvm但是有[nodist (Natural node version manager for windows)](https://github.com/marcelklehr/nodist)（记得添加bin目录到系统PATH中），然后跟nvm差不多的操作来安装nodejs。另外，Windows用户推荐在Powershell中操作。

2. 采用taobao的npm镜像，安装cnpm
如同gem镜像一样，taobao也有个npm镜像叫做[TAONPM](http://npm.taobao.org/)，没10分钟同步一次，杠杠滴。
每次指定镜像源太麻烦了，所以有了cnpm：
`npm install -g cnpm --registry=http://registry.npm.taobao.org`
下次再想从taobao镜像安装的时候，直接
`cnpm install -g *`
很方便。

##安装并初始化Hexo
1. 安装Hexo
搭建好了nodejs环境，安装模块就很简单了：
`npm install -g hexo`

2. 初始化
以我的Windows机器（POWERSHELL）为例：
`PS C:\Users\66beta> hexo init 66beta.github.io`
**注意：这里就在C:\Users\66beta目录下运行init，不然会出错，具体不知道为什么，以后再研究**
来到C:\Users\66beta\66beta.github.io目录下，就可以看到Hexo的初始化的文件了。
[hexo-generator-feed](https://github.com/hexojs/hexo-generator-feed)是一个hexo的插件，有了它才可以生成RSS，建议安装，在C:\Users\66beta\66beta.github.io目录下运行：
`npm install hexo-generator-feed --save`
然后，根据[Hexo官方文档](http://hexo.io/docs/configuration.html)来对_config.yml做一些配置。

##写文章并生成HTML
1. 添加一篇文章
`hexo new [layout] <title>`
比如：
`hexo new "Hello Hexo"`
在source\\_posts目录下就会有一个hello-hexo.md文件，用编辑器打开这个文件，编写markdown～

2. 生成HTML
编写完后，运行生成HTML的命令：
`hexo generate`

3. 预览
生成完HTML文件后，可以运行一个web服务来查看效果：
`hexo server`
保持开启状态，然后回去修改hello-hexo.md文件，刷新浏览器，修改即时呈现。

##完毕

以上只是记录下安装的流程，具体安装与使用请见[Hexo官网](http://hexo.io/)，下次有空研究下用traviCI和prose.io在线自动发布到Github看看。

自从换掉了Jekyll用上了Hexo，妈妈再也不同担心我不写博客了，So easy～




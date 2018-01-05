---
title: CSS小技巧/贴士
date: 2014-05-15 16:59:12
tags: css
categories: css
---
## 使用简写

比如：`margin-top:5px;margin-bottom:5px;margin-left:0;margin-right:0;`
可以简写为：`margin:5px 0;`
其他还有很多可以简写的，比如font的定义，具体参考W3C文档。

## 为选中的文字定义样式

比如：`::selection {color:#09c;}`
适用于现代浏览器，有时需加厂商前缀，如：`::-moz-selection{}`
这个功能在国外一些激进的网页设计博客上，2年前就开始使用了，<!-- more -->当然了人家环境好。

## 定义多个字体

5年前，还流行着：`font-family:arial,'宋体',sans-serif;`，网页上普遍都是‘宋体’
如今更多人选择：`font-family:arial,Microsoft YaHei','SimSun',sans-serif;`
对于中文字体，先检测电脑上是否有微软雅黑，没有则采用后面一个宋体，这样可以添加多个与设计相近的字体，尽量保持网页在不同平台上字体的统一性。

## 过渡transition写在默认样式中

假设要对`<a>`标签hover时改变文字颜色，并且添加trasition过渡效果，那么应当添加在：`a {color:#ccc;transition:color 0.5s ease-in-out;}`中
而不是这里：`a:hover {color:#096;}`，这样的话从hover切换回默认样式，也会有过渡效果～

## 文字在div中垂直居中

有一个div：`<div>我要垂直居中！</div>`
对div定义`div {height:100px;display:table-cell;vertical-align:middle;}`
原理是使div像td一样表现，这样就可以定义vertical-align了。

## 任意元素水平垂直居中

以图片为例：`<div><img src="sample.jpg" width="200" height="200"></div>`
div应当有个高度：`div {height:600px;position:relative;}`
对图片进行绝对定位：`div img {position:absolute;top:50%;left:50%;}`，这时只是图片的左上角位于div的正中心，需要添加`div img {position:absolute;top:50%;left:50%;margin-top:-100px;margin-left:-100px;}`
其中100px是取自图片的高宽度的一半，所以这个写法要求元素的高宽固定。

## 清除浮动

老生常谈，假设有：`<div><img src="a.jpg"><p>text here</p></div>`，其中<img>元素左浮动
那么需要为父容器`<div>`做清除浮动的设置，最最简洁的代码：`div:before,div:after{content:"";display:table;} div:after{clear:both;}`
原理就是通过设置:before和:after伪元素来清除浮动，有些版本还会加上行高、字号之类的，都是对此的增强。需要注意的是:before和:after在IE8及以上才支持。

## 用CSS做三角形

这个东西最早有一个老外的博客上看到，现在找不到了，找到一个比较早的是[CSS Tricks](http://css-tricks.com/snippets/css/css-triangle/)上的一篇文章。
其实代码很简单：`<div></div>`，先设置样式：`div{height:0;width:0;margin-bottom:1em;}`，然后如果是向上的：`border-left:5px solid transparent;border-right:5px solid transparent;border-bottom:5px solid #09c;`，其他方向同理～
如果要等边三角形，那么以向上箭头为例，需要计算底边宽度（左边宽+右边宽*0.866=8.66）：`border-bottom:9px solid #09c;`
当然了，也可以使用CSS3的transform来旋转角度，但是支持有限，且需要添加各产商前缀。

## 用box-shadow做纸张层叠效果

[jsFiddle地址](http://jsfiddle.net/66beta/ad4HG/)
{% jsfiddle ad4HG html,css,result %}

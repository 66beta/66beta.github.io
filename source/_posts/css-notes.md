---
title: CSS 笔记
date: 2014-05-15 16:59:12
tags:
  - css
categories:
  - 前端
---
> last update: 2018-01-10

主要记录一些平时很少用到，但可能会忘记的点。

## fastclick导致单选按钮需要点击两次

```css
label > * {
  pointer-events: none;
}
```

## 定义选中样式

如：

```css
::selection {color:#09c;}
```

<!-- more -->

## 移动端开发字体设置

```css
font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol';
```

## div文字垂直居中

```html
<div>我要垂直居中！</div>
```

```css
div { height: 100px; display: table-cell; vertical-align: middle; }
```


## 用CSS做三角形

```html
<div></div>
```

```css
div { height: 0; width: 0; margin-bottom: 1em; }
```

向上的：

```css
border-left: 5px solid transparent;
border-right: 5px solid transparent;å
border-bottom: 5px solid #09c;
```

等边三角形，需要计算底边宽度（左边宽+右边宽*0.866=8.66）：

```css
border-bottom: 9px solid #09c;
```

## 用box-shadow做纸张层叠效果

[jsFiddle地址](http://jsfiddle.net/66beta/ad4HG/)
{% jsfiddle ad4HG html,css,result %}

## 清除ios原生控件样式与点击时闪现的透明层

```css
* {
  -webkit-appearance: none;
  -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
}
```
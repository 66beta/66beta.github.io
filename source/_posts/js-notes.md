---
title: JS 笔记
date: 2018-01-10 10:46:45
tags:
  - js
categories:
  - 前端
---
记录js的知识点、跳坑指南。

## 声明提升（Hosing）

```js
a();
var a = 2;
function a(){
  console.log(1);
}
a();
/* 结果
1
Uncaught TypeError: foo is not a function
*/

// 实际执行时的代码
var foo; // 声明提升
function foo(){ // 声明提升
  console.log(1);
}
foo();
foo = 0; // 初始化赋值不提升
foo(); // 出错，此时 foo 是 0
```

<!-- more -->

## 闭包陷阱

典型案例，循环绑定事件

```html
<button>0</button>
<button>1</button>
<button>2</button>
<button>3</button>
<button>4</button>
```

```js
let btns = document.querySelectorAll('button')
function onClickButton (who) {
  console.log(who + ' clicked')
}
for (var i = 0; i < btns.length; i++) {
  btns[i].addEventListener('click', function () { // 匿名函数才可以传参
    onClickButton(i)
  }, false)
}

/* 结果
4 clicked
4 clicked
4 clicked
4 clicked
4 clicked
*/
```

想要达到预期效果，改成如下即可：

```js
let btns = document.querySelectorAll('button')
function onClickButton (who) {
  return function () { // 返回一个函数
    console.log(who + ' clicked')
  }
}
for (var i = 0; i < btns.length; i++) {
  btns[i].addEventListener('click', onClickButton(i), false) // 返回了函数，所以不需要匿名函数包裹了
}

/* 结果
0 clicked
1 clicked
2 clicked
3 clicked
4 clicked
*/
```

## ES6 解构

```js
// 获取元素
const array = [1, 2, 3, 4];
const [first, ,third] = array;
console.log(first, third); // 1 3

// 值交换
let a = 1;
let b = 2;
[a, b] = [b, a];
console.log(a, b); // 2 1

// 返回解构简写
function margin() {
  const left = 1, right = 2, top = 3, bottom = 4;
  return { left, right, top, bottom };
}
const { left, bottom } = margin();
console.log(left, bottom); // 1 4

// 参数匹配
const user = {firstName: '111', lastName: '222'};
function getFullName({ firstName, lastName }) {
  return `${firstName} ${lastName}`;
}
console.log(getFullName(user)); // 111 222

// 深度匹配
function settings() {
  return { display: { color: 'red' }, keyboard: { layout: 'querty'} };
}
const { display: { color: displayColor }, keyboard: { layout: keyboardLayout }} = settings();
console.log(displayColor, keyboardLayout); // red querty
```

## ES6 展开操作符

```js
// ES5
Math.max.apply(Math, [2,100,1,6,43]) // 100

// ES6
Math.max(...[2,100,1,6,43]) // 100
```
---
title: 《你不知道的JS(上)》学习笔记 第一部分
date: 2018-05-24 11:08:28
tags:
  - js
categories:
  - 前端
---

## 内建类型
  - string
  - nubmer
  - boolean
  - undefined
  - object (包括null)
  - symbol (ES6+)

## 类型转换
__Truthy&Falsy__
  - `Boolean(NaN) === false`
  - `Boolean([]) === true`
  - `Boolean({}) === true`

__等价__
```js
var a = [1,2,3]
var b = [1,2,3]
var c = "1,2,3"

a == c // true
b == c // true
a == b // false

[1,2,3].toString() // "1,2,3"
```
__不等价__
```js
var a = 42
var b = "foo"

a < b // false
a > b // false
a == b // false

Number("foo") // NaN
```
NaN既不大于其他值，也不小于其他值

<!-- more -->

## 变量
标识符必须以a-z，A-Z，$，或_开头。它可以包含任意这些字符外加数字0-9。

## Strict模式
使代码符合一组更安全和更合理的指导方针，代码对引擎有更强的可优化性。

## 立即调用表达式IIFE
```js
(function IIFE(){
  console.log( "Hello!" );
})();
// "Hello!"
```

## 闭包
```js
function makeAdder(x) {
  function add(y) {
    return y + x;
  };
  return add;
}
var plusOne = makeAdder( 1 );
plusOne( 3 ); // 4 <-- 1 + 3
```

## 模块
模块让你定义对外面世界不可见的私有实现细节（变量，函数），和对外面可访问的公有API。
```js
function User() {
  var username;
  var password;
  function doLogin(user,pw) {
    username = user;
    password = pw;
  }
  var publicAPI = {
    login: doLogin
  };
  return publicAPI;
}
// 创建实例
var fred = User();
fred.login('jack', '123');
```

## this标识符
```js
function foo() {
  console.log( this.bar );
}
var bar = "global";
var obj1 = {
  bar: "obj1",
  foo: foo
};
var obj2 = {
  bar: "obj2"
};

foo();            // "global"
obj1.foo();       // "obj1"
foo.call( obj2 ); // "obj2"
new foo();        // undefined
```
  - `foo()`最终在*非strict*模式中将*this*设置为全局对象；在*strict*模式中*this*将会是`undefined`
  - `obj1.foo()`将*this*设置为对象*obj1*
  - `foo.call(obj2)`将*this*设置为对象*obj2*
  - `new foo()`将*this*设置为一个新的空对象

## 原型
```js
var foo = {
  a: 42
};
// 创建 `bar` 并将它链接到 `foo`
var bar = Object.create( foo );
bar.b = "hello world";
bar.b; // "hello world"
bar.a; // 42 <-- 委托到 `foo`
```
## 作用域

LHS查询 `var a`
RHS查询 `a = 2`

### 欺骗

1. `eval`
```js
function foo(str, a) {
  eval( str );
  console.log( a, b );
}
var b = 2;
foo( "var b = 3;", 1 ); // 1 3
```
2. `with`
```js
function foo(obj) {
  with (obj) {
    a = 2; // o2时，隐式 var a
  }
}
var o1 = { a: 3 };
var o2 = { b: 3 };

foo( o1 );
console.log( o1.a ); // 2
foo( o2 );
console.log( o2.a ); // undefined
console.log( a ); // 2 泄漏到了全局作用域
```

### 性能
`eval` 和 `with`都不建议使用，且受严格模式限制。引擎不会优化`eval` 和 `with`，使用多会卡。

## 函数与块级作用域
```js
function doSomething(a) {
  function doSomethingElse(a) {
    return a - 1;
  }
  var b;
  b = a + doSomethingElse( a * 2 );
  console.log( b * 3 );
}
doSomething( 2 ); // 15
```
`b` 和 `doSomethingElse(..)` 对任何外界影响都是不可访问的，而是仅仅由 `doSomething(..)` 控制。它的功能和最终结果不受影响，但是这种设计将私有细节保持为私有的，这通常被认为是好的软件。

### 命名空间
```js
var MyReallyCoolLibrary = {
  awesome: "stuff",
  doSomething: function() {},
  doAnotherThing: function() {}
};
```

### 匿名函数命名
```js
setTimeout( function timeoutHandler(){
  console.log( "I waited 1 second!" );
}, 1000 );
// ES6 箭头函数
var timeoutHandler = () => console.log( "I waited 1 second!" );
```

### IIFE
```js
var a = 2;
(function IIFE( global ){
  var a = 3;
  console.log( a ); // 3
  console.log( global.a ); // 2
})( window );
console.log( a ); // 2
```
UMD模块
```js
var a = 2;
(function IIFE( def ){
  def( window );
})(function def( global ){
  var a = 3;
  console.log( a ); // 3
  console.log( global.a ); // 2
});
```
`def` 函数表达式在这个代码段的后半部分被定义，然后作为一个参数（也叫 `def`）被传递给在代码段前半部分定义的 `IIFE` 函数。最后，参数 `def` （函数）被调用，并将 `window` 作为 `global` 参数传入。

### 块级作用域
- `With`
- `try/catch`
  ```js
  try { throw 2 } catch(a) {
    console.log( a ); // 2
  }
  console.log( a ); // ReferenceError
  ```
- `let`
- `const`

### 垃圾回收
```js
// bad
function process(data) { .. }
var someReallyBigData = { .. };
process( someReallyBigData );
var btn = document.getElementById( "my_button" );
btn.addEventListener( "click", function click(evt){
  console.log("button clicked");
}, /*capturingPhase=*/false );

// 优化
function process(data) { ..}
// 运行过后，任何定义在这个块中的东西都可以消失了
{
  let someReallyBigData = { .. };
  process( someReallyBigData );
}
var btn = document.getElementById( "my_button" );
btn.addEventListener( "click", function click(evt){
  console.log("button clicked");
}, /*capturingPhase=*/false );
```

## 提升

```js
// 函数优先
foo(); // 1
var foo; // var 被抛弃
function foo() {
  console.log( 1 );
}
foo = function() {
  console.log( 2 );
};

// 函数声明覆盖
foo(); // 3
function foo() {
  console.log( 1 );
}
var foo = function() {
  console.log( 2 );
};
function foo() {
  console.log( 3 );
}
```

## 闭包
```js
// 错误 1
for (var i=1; i<=5; i++) {
  setTimeout( function timer(){
    console.log( i ); // 打印6个6
  }, i*1000 );
}
// 错误 2
for (var i=1; i<=5; i++) {
  (function(){
    setTimeout( function timer(){
      console.log( i );
    }, i*1000 );
  })();
}
// 正确
for (var i=1; i<=5; i++) {
  (function(){
    var j = i; // IIFE作用域内部变量
    setTimeout( function timer(){
      console.log( j );
    }, j*1000 );
  })();
}
// 优雅
for (var i=1; i<=5; i++) {
  (function(j){
    setTimeout( function timer(){
      console.log( j );
    }, j*1000 );
  })( i ); // 传参方式
}
// ES6
for (let i=1; i<=5; i++) {
  setTimeout( function timer(){
    console.log( i );
  }, i*1000 );
}
```

### 基于闭包的模块
```js
function CoolModule() {
  var something = "cool";
  var another = [1, 2, 3];
  function doSomething() {
    console.log( something );
  }
  function doAnother() {
    console.log( another.join( " ! " ) );
  }
  return {
    doSomething: doSomething,
    doAnother: doAnother
  };
}
var foo = CoolModule();
foo.doSomething(); // cool
foo.doAnother(); // 1 ! 2 ! 3
```
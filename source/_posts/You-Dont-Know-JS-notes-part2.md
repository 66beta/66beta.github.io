---
title: 《你不知道的JS(上)》学习笔记 第二部分
date: 2018-05-24 11:08:28
tags:
  - js
categories:
  - 前端
---

## 关于this

在函数被调用时建立的一个绑定，它指向 __什么__ 是完全由函数被调用的调用位置来决定的。

### call绑定
```js
function identify() {
  return this.name.toUpperCase();
}
function speak() {
  var greeting = "Hello, I'm " + identify.call( this );
  console.log( greeting );
}
var me = { name: "Kyle" };
var you = { name: "Reader" };

identify.call( me ); // KYLE
identify.call( you ); // READER

speak.call( me ); // Hello, I'm KYLE
speak.call( you ); // Hello, I'm READER
```

<!-- more -->

## this解析

调用位置：函数在代码中被调用的位置（__不是被声明的位置__）。

### 默认绑定
```js
// 严格模式下不允许默认绑定this到全局
function foo() {
  "use strict";
  console.log( this.a );
}
var a = 2;
foo(); // TypeError: `this` is `undefined`

// 下面严格模式与foo()调用位置无关
function foo() {
  console.log( this.a );
}
var a = 2;
(function(){
  "use strict";
  foo(); // 2
})();
```

### 隐式绑定

引用链的最后一层是影响调用位置的：

```js
function foo() {
  console.log( this.a );
}

var obj2 = {
  a: 42,
  foo: foo
};

var obj1 = {
  a: 2,
  obj2: obj2
};

obj1.obj2.foo(); // 42
```

隐式丢失，绑定到默认全局，或严格模式下的`undefined`

```js
function foo() {
  console.log( this.a );
}
var obj = {
  a: 2,
  foo: foo
};
var bar = obj.foo; // 函数引用！
var a = "oops, global"; // `a` 也是一个全局对象的属性

bar(); // "oops, global"
```

传入回调函数：

```js
function foo() {
  console.log( this.a );
}
function doFoo(fn) {
  // fn引用foo
  fn(); // <-- 调用位置!
}
var obj = {
  a: 2,
  foo: foo
};
var a = "oops, global"; // a 是全局对象的属性

doFoo( obj.foo ); // "oops, global"
```

### 显式绑定

```js
function foo() {
  console.log( this.a );
}
var obj = {
  a: 2
};

foo.call( obj ); // 2
```
通过 `foo.call(..)` 强制把 `this` 绑定到 `obj`

如果传递一个简单基本类型值（`string boolean number`）作为 this 绑定，那么这个基本类型值会被包装在它的对象类型中（分别是 `new String(..)`，`new Boolean(..)`，或 `new Number(..)`）。称为“装箱（boxing）”。

1. 硬绑定
```js
function foo() {
  console.log( this.a );
}
var obj = {
  a: 2
};
var bar = function() {
  foo.call( obj ); // 硬绑定
};

bar(); // 2
setTimeout( bar, 100 ); // 2
// 不可以被覆盖
bar.call( window ); // 2
```

__硬绑定的应用：__

- 包裹函数，负责接收参数并返回值
  ```js
  function foo(something) {
    console.log( this.a, something );
    return this.a + something;
  }
  var obj = { a: 2 };
  var bar = function() {
    return foo.apply( obj, arguments );
  };

  var b = bar( 3 ); // 2 3
  console.log( b ); // 5
  ```

- 辅助函数
  ```js
  function foo(something) {
    console.log( this.a, something );
    return this.a + something;
  }
  // 简单的 `bind` 帮助函数
  function bind(fn, obj) {
    return function() {
      return fn.apply( obj, arguments );
    };
  }
  var obj = { a: 2 };
  var bar = bind( foo, obj );

  var b = bar( 3 ); // 2 3
  console.log( b ); // 5
  ```

硬绑定是一个如此常用的模式，它已作为 ES5 的内建工具 `Function.prototype.bind`
```js
function foo(something) {
  console.log( this.a, something );
  return this.a + something;
}
var obj = {
  a: 2
};
var bar = foo.bind( obj );

var b = bar( 3 ); // 2 3
console.log( b ); // 5
```

2. api调用的上下文(context)

```js
function foo(el) {
  console.log( el, this.id );
}
var obj = {
  id: "awesome"
};
// 使用 `obj` 作为 `this` 来调用 `foo(..)`
[1, 2, 3].forEach( foo, obj ); // 1 awesome  2 awesome  3 awesome
```

待续...
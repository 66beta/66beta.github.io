---
title: Javascript中获取日期要注意时区
date: 2018-09-03 15:49:07
tags:
 - js
categories:
  - 前端
---

> 记一个无知与粗心引发的BUG

需要获取当前日期，格式为`2018-09-03`，于是想当然地写了：
```js
new Date().toJSON() // "2018-09-03T07:52:51.903Z"
new Date().toJSON().slice(0, 10) // "2018-09-03"
```

如果不熟悉Date对象的方法，这里就有个天坑了，`toJSON()`方法获取的是格林尼治标准时间（UTC），中国时区是东八区领先8小时，即平时很多地方看到的`+0800`。

<!-- more -->

问题就在这里，如果你是白天正常工作时间(09:00 ~ 18:00)写代码，可能发现不了这个bug，因为获取的是日期`2018-09-03`，这个时间段不会有影响啊。

如果是早上8点前，问题就出现了。假设你设备上当前是`2018-09-03 06:30:00`，UTC日期其实还在你的昨天`2018-09-02`：
```js
new Date('2018-09-03 06:30:00').toJSON() // "2018-09-02T22:30:00.000Z"
new Date('2018-09-03 06:30:00').toJSON().slice(0, 10) // "2018-09-02"
```

更悲催的是，如果没意识到是这个问题，在(09:00 ~ 18:00)工作时间断排查，就要怀疑人生了...

那么正确姿势呢？

```js
// '01'.slice(-2) => '01'
// '001'.slice(-2) => '01'
getTodayDate () {
  let d = new Date()
  return d.getFullYear() + '-' + ('0' + (d.getMonth() + 1)).slice(-2) + '-' + ('0' + d.getDate()).slice(-2)
}
```


提高自己姿势水平，有空多看看 [MDN的文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date)

Monentjs太重，自己维护一个Date工具库可能是个好主意...

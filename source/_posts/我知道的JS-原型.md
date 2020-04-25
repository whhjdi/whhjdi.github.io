---
title: 我知道的JS-原型
date: 2018-04-04 15:47:45
tags: [JavaScript]
categories: ["Javascript"]
cover_img: https://ws1.sinaimg.cn/large/006tNbRwly1fy1lym00xbj313i0m8npd.jpg
---

# 原型

每个函数都有 prototype 属性，除了 Function.prototype.bind()这个特殊的函数

每个对象都有 **proto** 属性，指向了创建该对象的构造函数的原型。其实这个属性指向了 [[prototype]]，但是 [[prototype]] 是内部属性，我们并不能访问到，所以使用 `_proto_`来访问。

<!-- more -->

## 原型链

对象可以通过 `_proto_` 来寻找不属于该对象的属性，`_proto_` 将对象连接起来组成了原型链。
Object 是所有对象的爸爸，所有对象都可以通过 `_proto_` 找到它
Function 是所有函数的爸爸，所有函数都可以通过 `_proto_` 找到它
Function.prototype 和 Object.prototype 是两个特殊的对象，他们由引擎来创建。
除了以上两个特殊对象，其他对象都是通过构造器 new 出来的
函数的 prototype 是一个对象，也就是原型。
对象的 `_proto_` 指向原型， `_proto_` 将对象和原型连接起来组成了原型链。
数组就是数据的有序集合
数组就是原型链中有 Array.prototype 的对象
伪数组就是原型链中没有 Array.prototype 的对象
比如：arguements 对象和 document.querySelectorAll('div')返回的对象

## new

1. 新生成了一个对象
2. 链接到原型
3. 绑定 this
4. 返回新对象

```javascript
function _new(/* 构造函数 */ constructor, /* 构造函数参数 */ params) {
  // 将 arguments 对象转为数组
  var args = [].slice.call(arguments);
  // 取出构造函数
  var constructor = args.shift();
  // 创建一个空对象，继承构造函数的 prototype 属性
  var context = Object.create(constructor.prototype);
  // 执行构造函数
  var result = constructor.apply(context, args);
  // 如果返回结果是对象，就直接返回，否则返回 context 对象
  return typeof result === "object" && result != null ? result : context;
}

// 实例
var actor = _new(Person, "张三", 28);
```

## 对象的类型

使用 instanceof 可以正确的判断对象的类型，因为内部机制是通过判断对象的原型链中是不是能找到类型的 prototype

```javascript
function instanceof(left, right) {
    // 获得类型的原型
    let prototype = right.prototype
    // 获得对象的原型
    left = left.__proto__
    // 判断对象的类型是否等于类型的原型
    while (true) {
        if (left === null)
            return false
        if (prototype === left)
            return true
        left = left.__proto__
    }
}
```

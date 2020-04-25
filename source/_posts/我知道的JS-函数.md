---
title: 我知道的JS-函数
date: 2018-04-14 17:40:47
tags: [JavaScript]
categories: ["Javascript"]
cover_img: https://ws3.sinaimg.cn/large/006tNbRwly1fy1ly9uclzj30xc0kux1b.jpg
---

# 函数

函数就是一段可以反复调用的代码块.函数还能接受输入的参数,不同的参数会返回不同的值.
具名函数，匿名函数，箭头函数

## this & arguments

this 就是 call 一个函数时,传入的第一个参数（一般是对象）
call 的其他参数统称为 arguments

```javascript
fn() 里面的 this 就是 window
fn() 是 strict mode，this 就是 undefined
a.b.c.fn() 里面的 this 就是 a.b.c
new F() 里面的 this 就是新生成的实例
() => console.log(this) 里面 this 跟外面的 this 是一样的
```

<!-- more -->

## 闭包

闭包的定义很简单：函数 A 返回了一个函数 B，并且函数 B 中使用了函数 A 的变量，函数 B 就被称为闭包。

```javascript
function A() {
  let a = 1;
  function B() {
    console.log(a);
  }
  return B;
}
```

经典面试题，循环中使用闭包解决 var 定义函数的问题

```javascript
for (var i = 1; i <= 5; i++) {
  setTimeout(function timer() {
    console.log(i);
  }, i * 1000);
}
```

首先因为 setTimeout 是个异步函数，所有会先把循环全部执行完毕，这时候 i 就是 6 了，所以会输出一堆 6。

解决办法两种，第一种使用闭包

```javascript
for (var i = 1; i <= 5; i++) {
  (function(j) {
    setTimeout(function timer() {
      console.log(j);
    }, j * 1000);
  })(i);
}
```

第二种就是使用 setTimeout 的第三个参数

```javascript
for (var i = 1; i <= 5; i++) {
  setTimeout(
    function timer(j) {
      console.log(j);
    },
    i * 1000,
    i
  );
}
```

第三种就是使用 let 定义 i 了

```javascript
for (let i = 1; i <= 5; i++) {
  setTimeout(function timer() {
    console.log(i);
  }, i * 1000);
}
```

因为对于 let 来说，他会创建一个块级作用域

## call/apply/bind

fn.call(asThis,p1,p2) 是正常的调用方式
不确定参数的个数时，用 apply,fn.apply(asThis,params)
bind 返回一个新的函数

## mixin 混入

将一个对象的属性赋值给另一个对象

```javascript
var mixin = function(a, b) {
  for (let key in b) {
    a[key] = b[key];
  }
};
```

## curry 柯里化

把接收多个参数的函数变成接收一个单一参数的函数，并且返回接收剩余参数的函数的结果的新函数
(将真实的计算放到最后做)
柯里化之前

```javascript
var add = function(a, b) {
  return a + b;
};
```

柯里化之后

```javascript
var add = function(a) {
  return function(b) {
    return a + b;
  };
};
```

## 高阶函数

在数学和计算机科学中，高阶函数是至少满足下列一个条件的函数：
接受一个或多个函数作为输入：forEach sort map filter reduce
输出一个函数：lodash.curry
不过它也可以同时满足两个条件：Function.prototype.bind
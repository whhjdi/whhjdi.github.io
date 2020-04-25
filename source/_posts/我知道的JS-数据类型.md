---
title: 我知道的JS-数据类型
date: 2018-04-04 15:17:42
tags: [JavaScript]
categories: ["Javascript"]
cover_img: https://ws1.sinaimg.cn/large/006tNbRwly1fy1lzl0xf9j31040pjb2a.jpg
---

# 前言

> 原创之处并不优秀,优秀之处并非原创.
> 良好 JavaScript 基础是前端工程师的赖以生存的基石,有了扎实的 js 基础才能走得更远.
> 目前我的学习资料有:JavaScript 高级程序设计,阮一峰的 js 教程,你不知道的 JavaScript 上中下三卷,其他优秀博客等......

<!-- more -->

## 数据类型

七种数据类型

```javascript
string, number, boolean, symbol, undefined, null, object;
```

### 类型转换

任意类型转字符串

String（）

```javascript
String(1);
//"1"
String(true);
//"true"
String(null);
//"null"
String(undefinded);
//"undefined"
String({});
//"[object Object]"
```

x.toString()

```javascript
(1).toString()
//"1"
true.toString()
//"true"
null.toString()
//报错
undefined.toString()
//报错
{}.toString()
//报错
({}).toString()
//"[object Object]"
```

x+’’ 小技巧

```javascript
1 + "";
//"1"
true + "";
//"true"
null + "";
//"null"
undefined + "";
//"undefined"
{
}
+"";
//0
var o = {};
0 + "";
//"[object Object]"
```

任意类型转数字

- Number(x)
- parseInt(x,10)
- parseFloat(x)
- x-0
- +x

任意类型转布尔

- Boolean(x)
- !!x
- 5 个 falsy 值:

```javascript
0, "", null, undefined, NaN;
```

### Typeof

typeof 对于基本类型，除了 null 都可以显示正确的类型

```javascript
typeof 1; // 'number'
typeof "1"; // 'string'
typeof undefined; // 'undefined'
typeof true; // 'boolean'
typeof Symbol(); // 'symbol'
typeof a; // b 没有声明，但是还会显示 undefined
```

typeof 对于对象，除了函数都会显示 object

```javascript
typeof []; // 'object'
typeof {}; // 'object'
typeof console.log; // 'function'
```

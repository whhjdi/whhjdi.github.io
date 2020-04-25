---
title: 我知道的JS-其他
date: 2018-05-13 10:53:13
tags: [JavaScript]
categories: ["Javascript"]
cover_img: https://ws1.sinaimg.cn/large/006tNbRwly1fy1ly2z45wj31320rshdt.jpg
---

# 其他

## Map、FlatMap 和 Reduce

map 遍历原数组，处理每个元素，返回一个新数组

```javascript
["1", "2", "3"].map(parseInt);
//返回十进制的数，无法转换为数值则返回 NaN
//  parseInt('1', 0) -> 1
//  parseInt('2', 1) -> NaN
//  parseInt('3', 2) -> NaN
```

<!--more-->

flatMap

```javascript
var arr1 = [1, 2, 3, 4];

arr1.map(x => [x * 2]);
// [[2], [4], [6], [8]]

arr1.flatMap(x => [x * 2]);
// [2, 4, 6, 8]

// only one level is flattened
arr1.flatMap(x => [[x * 2]]);
// [[2], [4], [6], [8]]

//等价于
arr1.reduce((acc, x) => acc.concat([x * 2]), []);
```

彻底扁平化数组

```javascript
const flattenDeep = arr =>
  Array.isArray(arr)
    ? arr.reduce((a, b) => [...a, ...flattenDeep(b)], [])
    : [arr];
```

## Proxy 和 Reflect

基本用法

```javascript
let p = new Proxy(target, handler);
// `target` 代表需要添加代理的对象
// `handler` 用来自定义对象中的操作
```

实现一个数据绑定和监听

```javascript
let onWatch = target => {
  let handler = {
    get(target, property, receiver) {
      return Reflect.get(target, property, receiver);
    },
    set(target, property, value, receiver) {
      return Reflect.set(target, property, value, receiver);
    }
  };
  return new Proxy(target, handler);
};
```

## 精度问题

JS 采用 IEEE754 双精度版本，所以会有精度问题
0.1+0.2 ！= 0.3

```javascript
parseFloat((0.1 + 0.2).toFixed(10));
```

## (a ==1 && a== 2 && a==3) 可能为 true 吗？

```javascript
//第一种

var temp = 1;
Object.defineProperty(window, "a", {
  get() {
    return temp++;
  }
});

//第二种

var a = {
  temp: 1,
  valueOf() {
    return this.temp++;
  }
};
//第三种

var a = {
  temp: 1,
  toString() {
    return this.temp++;
  }
};
```

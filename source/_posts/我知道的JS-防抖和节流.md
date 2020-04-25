---
title: 我知道的JS-防抖和节流
date: 2018-04-05 22:34:16
tags: [JavaScript]
categories: ["Javascript"]
cover_img: https://ws4.sinaimg.cn/large/006tNbRwly1fy1m1pxemoj315o0q21kx.jpg
---

# 防抖和节流

之前一直不明白或者弄混两者，也是因为没有遇到这种需求
作用：都是防止函数多次调用
区别：防抖动是将多次执行变为最后一次执行(将多个信号合并为一个信号)，节流是将多次执行变成每隔一段时间执行。

<!--more-->

## 防抖(debounce)

使用场景
input 输入的格式验证
在一个滚动事件中做一些复杂计算或者实现一个按钮的防二次点击操作

延迟执行的防抖函数

```javascript
function debounce(fn,time){
  let timer = null
  return function(...args){
    clearTimeout(timer)
    setTimeout(()=>{
      fn.call(this,...args)
    },time)
  }
}
```

## 节流(throttle)

使用场景
用在比 input,keyup 触发更频繁的事件中，比如，resize,touchmove,scroll 等
适合用于动画相关的场景

```javascript
function throttle(fn,time){
  let flag = true
  return function(...args){
    if(!flag) return ;
    flag = false
    setTimeout(()=>{
      fn.call(this,...args)
      flag = true
    },time)
  }
}
```

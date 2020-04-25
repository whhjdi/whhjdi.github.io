---
title: 我知道的JS-异步
date: 2018-04-09 15:40:17
tags: [JavaScript]
categories: ["Javascript"]
cover_img: https://ws1.sinaimg.cn/large/006tNbRwly1fy1lz7rnkvj312w0mhkjm.jpg
---

# 异步

## Promise

Promise 是 Es6 新增的语法，为了解决回调地狱的问题
Promise 有三种状态，pending(初始状态)，可以通过函数 resolve 和 reject 把状态变为 resolved 或者 rejected，状态只能改变一次

```javascript
function xxx(){
    return new Promise(function(resolve,reject){
        setTimeout(()=>{
            resolve()
            //或者
            //reject()
        },1000)
    })
}
xxx().then(......)
```

<!--more-->

## async/await

ES8 引入的 async 函数，可以更加方便的处理异步

一个函数如果加上 async ，那么该函数就会返回一个 Promise

```javascript
function returnPromise() {
  return new Promise(function(resolve, reject) {
    setTimeout(() => {
      console.log("finish");
      resolve("muxue");
    }, 3000);
  });
}

//之前我们可以这样写
// returnPromise().then(result => {
//     console.log(result)
// });

async function test() {
  let result = await returnPromise(); // 注意只有控制台支持顶级作用域的 await，JS 文件里的 await 只能写在 async 函数里
  console.log(result);
}
test();
```

## generator

```javascript
function fn(a, b) {
  return a + b;
}
function* gen(x) {
  console.log(x);
  let y = yield fn(x, 100) + 3;
  console.log(y);
  return 200;
}
let g = gen(1);
console.log(g.next().value);
// 1  104
console.log(g.next().value);
//undefined 200
```

当调用 g.next()时，gen 函数开始执行，执行到第一个 yield 为止，并把 yield 表达式的值作为状态对象的值。更具体一点，上例先输出 x 也就是 1，然后执行 fn(x, 100) 输出 fn..并返回 101， 然后加 3。这时候停止执行，把结果 104 赋值给状态对象 g，g 的结果变 {value: 104, done: false}。需要注意，yied 表达式的优先级极其低，yield fn(x,100) + 3 相当于 yield (fn(x,100) + 3)
第二次执行 g.next()的时候，代码由上次暂停处开始执行，但此时 yield 表达式的值并不是使用刚刚计算的结果，而是使用 g.next 的参数 undefined， 所以 y 的值变为 undefined，输出 undeined。执行到 return 200 时，状态对象知道执行结束了，会把 return 的 200 赋值到状态对象，结果为 { value: 200, done: true }
如何把刚刚计算的中间值 103 给下个 yield 来用呢？好问题，我们可以这样

```javascript
g.next(g.next().value);
```

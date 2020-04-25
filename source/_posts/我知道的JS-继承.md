---
title: 我知道的JS-继承
date: 2018-04-10 22:49:21
tags: [JavaScript]
categories: ["Javascript"]
cover_img: https://ws3.sinaimg.cn/large/006tNbRwly1fy1m17r3gyj30qe0ipwrf.jpg
---

## 继承

继承可以使得子类具有父类的各种属性和方法
在高程中介绍了好多种，但是完美的就这一种，所以，就理解好这一种就可以了

### ES5 中的继承

```javascript
function Human(name) {
  this.name = name;
}
Human.prototype.run = function() {
  console.log("i can run");
};
function Man(name) {
  Human.call(this, name);
  this.gender = "男";
}
//如果 Man.prototype.__proto__ = Human.prototype就实现了继承
// 但是这样写是不允许的,
//所以
Man.prototype = Object.create(Human.prototype);
Man.prototype.constructor = Man;
```

<!--more-->

### ES6 中的继承

```javascript
class Human {
  constructor(name) {
    this.name = name;
  }
  run() {
    console.log("i can run");
  }
}
class Man extends Human {
  constructor(name) {
    super();
    this.gender = "男";
  }
  fight() {
    console.log("fight");
  }
}
```

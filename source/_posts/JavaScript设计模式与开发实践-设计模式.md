---
title: JavaScript设计模式与开发实践-设计模式
date: 2019-06-11 19:11:27
tags: [JavaScript]
cover_img: https://i.loli.net/2019/08/21/BlydImUieGf6qRk.jpg
feature_img:
description:
keywords:
---

## 设计模式

未完待续。。。。。。
总结 js 中常见的 14 种设计模式

### 单例模式

> 保证一个类只有一个实例，并提供一个访问它的全局访问点

要实现一个标准的单例模式其实就是用一个变量来标志当前是否已经为某个类创建过实例对象，如果没有就创建，如果有就在下一个获取该类的实例时，直接返回之前创建的对象

```js
var Singleton = function() {
  this.name = name;
  this.instance = null;
};
Singleton.prototype.getName = function() {
  alert(this.name);
};
Singleton.getInstance = function(name) {
  if (!this.instance) {
    this.instance = new Singleton(name);
  }
  return this.instance;
};
```

#### 减少全局变量的使用

1. 使用命名空间

   ```js
   var nameSpace = {
     a: function() {
       alert(1);
     },
     b: function() {
       alert(2);
     }
   };
   ```

2. 使用闭包封装私有变量

   ```js
   var user = (function() {
     var _name = "muxue";
     var _age = 18;
     return {
       getUserInfo: function() {
         return _name + "-" + _age;
       }
     };
   })();
   ```

#### 惰性单例

惰性单例是指在需要的时候才创建对象，在开发中非常有用

```js
//实现一个惰性单例
//把创建对象和管理单例分别放在两个不同的方法中
//管理单例的方法
var getSingle = function(fn) {
  var result;
  return function() {
    return result || fn.apply(this, arguments);
  };
};
//创建登录窗口的方法
var createLogin = function() {
  var div = document.createElement("div");
  div.innerHTML = "我是登陆窗口";
  div.style.display = "none";
  document.body.appendChild(div);
  return div;
};
var createSingleLogin = getSingle(createLogin);

document.getElementById("loginBtn").onClick = function() {
  var login = createSingleLogin();
  login.style.display = "block";
};
```

### 策略模式

> 策略模式的定义是： 定义一系列的算法，把他们一个个封装起来，并且使他们可以相互替换
> 策略模式的目的就是将算法的使用和算法的实现分离开来

1. 需求：根据员工的评级来发奖金

   ```js
   //不使用策略模式
   var calculateBonus = function(performanceLevel, salary) {
     if (performanceLevel === "S") {
       return salary * 4;
     }
     if (performanceLevel === "A") {
       return salary * 3;
     }
     if (performanceLevel === "B") {
       return salary * 2;
     }
   };
   calculateBonus("B", 20000);

   //通过策略模式可以消除程序中大片的条件分支语句
   var strategies = {
     S: function(salary) {
       return salary * 4;
     },
     A: function(salary) {
       return salary * 3;
     },
     B: function(salary) {
       return salary * 2;
     }
   };

   var calcBonus = function(level, salary) {
     return strategies[level](salary);
   };
   ```

2. 使用策略模式实现缓动动画

3. 使用策略模式实现一个表单验证

### 代理模式

> 代理模式是为一个对象提供一个代用品或占位符，以便控制对他的访问

### 迭代器模式

### 发布订阅模式

### 命令模式

### 组合模式

### 模板方法模式

### 享元模式

### 职责链模式

### 中介者模式

### 装饰者模式

### 状态模式

### 适配器模式

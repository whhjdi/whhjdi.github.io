---
title: JavaScript设计模式与开发实践-基础部分
date: 2019-06-10 11:20:14
tags: [JavaScript]
cover_img: https://i.loli.net/2019/08/21/BlydImUieGf6qRk.jpg
feature_img:
description:
keywords:
---

## 面向对象的 javaScript

### 在动态类型语言的面向对象设计中，鸭子类型的概念非常重要

> 如果他走起路来像鸭子，叫起来也是鸭子，那么他就是鸭子

也就是说： 一个对象若有 push 和 pop 方法，并且提供了正确的实现，他就可以被当做栈来使用

### 多态

多态的思想实际上是吧“做什么”和“谁去做”分离开来。JavaScript 中的多态性是与生俱来的，就是说动物能否发声只取决于他有没有 makeSound 方法，不取决于他是某种类型的对象

### 封装

封装的目的是把信息隐藏，而在 js 中一般通过函数作用域来实现封装的特性
封装可以分为：封装数据，封装类型，封装实现，封装变化等
一般而言，封装类型是通过抽象类和接口来进行的，比如工厂方法模式，组合模式等

### 继承

1. 克隆是创建对象的手段，通过`Object.create()`可以克隆一个对象

   ```js
   Object.create = Object.cteate || function(obj){
       var F = funciton(){}
       F.prototype = obj
       return new F()
   }
   ```

2. 在 js 中得到一个对象并不是通过实例化类，而是找到一个对象作为原型并克隆它
   当我们显式的调用`var a = new Object()`或者`var b = {}`时，js 引擎就会从 Object.prototype 上克隆一个对象出来，最终我们得到的就是这个对象

3. new 运算的过程

   ```js
   function Person() {
     this.name = name;
   }

   Person.prototype.getName = function() {
     return this.name;
   };

   var objectFactory = function() {
     var obj = {};
     Constructor = [].shift.call(arguments);
     obj.__proto__ = Constructor.prototype;
     var ret = Constructor.apply(obj, arguments);
     return typeof ret === "object" ? ret : obj;
   };
   ```

4. 原型继承
   js 最初的对象都是通过继承`Object.prototype`而来的，但是对象构造器的原型并不局限于它，我们可以动态的指向其他对象，比如说，当对象 a 需要 b 的能力时，我们可以把对象 a 的构造器的原型指向 b，从而达到继承的效果

   ```js
   //最常见的原型继承
   var obj = { name: "muxue" };
   function A() {}
   A.prototype = obj;

   var a = new A();
   console.log(a.name); // muxue
   ```

   - 首先通过遍历对象 a 中的所有属性，但是没找到 name 这个属性
   - 然后查找 name 属性的请求被委托给对象 a 的构造器的原型，也就是 A.prototype,也就是 a`__proto__`指向的对象,也就是 obj
   - 这样就在 obj 中找到了 name 属性，返回了他的值'muxue'

5. 当我们需要得到一个`类`继承另外一个`类`的效果是，怎么实现呢？

   ```js
   var A = function() {};
   A.prototype = { name: "muxue" };

   var B = function() {};
   B.prototype = new A();

   var b = new B();

   console.log(b.name); //muxue
   ```

   - 首先尝试遍历 b 对象中的私有属性，但是找不到 name
   - 然后在 b 的构造器的原型上找 name，也就是 b.`__proto__`指向的对象`B,prototype`,也就是`new A()`创建出来的对象，但是也没有找到
   - 于是就从 `new A()`这个对象的构造器的原型上找，也就是 A.prototype,然后输出'muxue'

   还有一种情况就是都不存在 name 属性，最终会输出 'undefined'

## this、call 和 apply

### this

this 在 js 中总是指向一个对象，它具体的指向是在运行时基于函数的执行环境动态绑定的，而不是被声明时的环境，通常来说 this 的指向可以大致分为以下 4 种情况

- 作为对象的方法调用(此时 this 指向该对象)

```js
var obj = {
  a: 1,
  getA: function() {
    console.log(this.a);
  }
};
obj.getA(); //输出 1
```

- 作为普通函数调用(this 指向全局对象，在浏览器中就是 window 对象)

```js
window.name = "globalname";

function getName() {
  console.log(this.name);
}
console.log(getName()); //输出 globalname
//或者
var myObj = {
  name: "muxue",
  getName: function() {
    console.log(this.name);
  }
};

var getName = myObj.getName;
console.log(getName());
```

- 构造器调用(通常情况下构造器里的 this 就指向 new A()返回的这个对象)

```js
var MyClass = function() {
  this.name = "muxue";
};
var obj = new MyClass();
alert(obj.name); //muxue
```

- `Function.prototype.call`或者`Function.prototype.apply`调用

```js
var obj1 = {
  name: "sven",
  getName: function() {
    return this.name;
  }
};
var obj2 = { name: "anne" };
console.log(obj1.getName()); //sven
console.log(obj1.getName.call(obj2)); // anne
```

### call 和 apply

如果传入的第一个参数为 null ,那么函数体内的 this 就只想默认的宿主对象，在浏览器中就是 window
在严格模式中还是 null
使用它们可以很方便的改变 this 的指向

### bind 的模拟实现

```js
Function.prototype.bind = funciton(){
    var self = this
    context = [].shift.call(arguments)
    args = [].slice.call(arguments)
    return function (){
        self.apply(context,args.concat([].slice.call(arguments)))
    }
}
```

## 闭包和高阶函数

### 闭包

1. 闭包的形成和`变量的作用域`以及`变量的生存周期`密切相关

   - 在函数中搜索一个变量时，如果该函数没有这个变量，那么就会随着代码的执行环境创建的作用域链向外层搜索，一直到搜索到全局对象为止，变量的搜索是从内到外的

   - 全局变量的生存周期是永久的，除非我们主动销毁。对象 var 生命的局部变量来说，当退出函数时，这些局部变量就是去了价值，他们就会随着函数调用的结束而销毁

   - 看下边的例子，当退出函数后，f 返回了一个匿名函数引用，并且可以访问到 func()被调用时的环境，而且局部变量 a 就在这个环境里，由于 a 所处的坏境还能被外界访问,那么 a 就有了不被销毁的理由，这里就产生了一个闭包结构

   ```js
   var func = function() {
     var a = 1;
     return function() {
       a++;
       alert(a);
     };
   };

   var f = func();
   f(); //2
   f(); //3
   f(); //4
   ```

   - 现在可以实现一个判断类型的函数

   ```js
   var Type = {};
   for (var i = 0, type; (type = ["String", "Array", "Number"][i++]); ) {
     (function(type) {
       Type["is" + type] = function(obj) {
         return Object.prototype.toString.call(obj) === "[object " + type + "]";
       };
     })(type);
   }
   Type.isArray([]);
   Type.isString("str");
   ```

2. 闭包的作用

   - 封装变量

   ```js
   var cache = {}; //缓存结果，如果之前计算过结果直接返回
   var mult = function() {
     var args = Array.prototype.join.call(arguments, ",");
     console.log(args);
     if (cache[args]) {
       return cache[args];
     }
     var a = 1;
     for (var i = 0, l = arguments.length; i < l; i++) {
       a = a * arguments[i];
     }
     return (cache[args] = a);
   };
   ```

   现在 cache 只在 mult 函数中被使用，那么就没必要放在全局作用域下

   ```js
   var mult = (function() {
     var cache = {};
     var calc = function() {
       var a = 1;
       for (var i = 0, l = arguments.length; i < l; i++) {
         a = a * arguments[i];
       }
       return a;
     };
     return function() {
       var args = Array.prototype.join.call(arguments, ",");
       console.log(args);
       if (cache[args]) {
         return cache[args];
       }
       return (cache[args] = calc.apply(null, arguments));
     };
   })();
   ```

   - 延续局部变量的寿命

   ```js
   //当report执行完毕后，img就会被销毁，而此时http请求可能还没有发送
   var report = function(src) {
     var img = new Image();
     img.src = src;
   };
   report("http://xxx.com/getUserInfo");
   //通过闭包，我们就解决了这种问题
   var report = (function() {
     var imgs = [];
     return function(src) {
       var img = new Image();
       imgs.push(img);
       img.src = src;
     };
   })();
   ```

3. 闭包与内存管理
   我们通过闭包让一些局部变量在函数执行结束后不会被销毁，这和我们直接在全局变量中生命对内存的影响是一致的，这并不是内存泄漏
   但是使用闭包的同时比较容易形成循环引用，如果闭包的作用域链存在一些 DOM 节点，就有可能造成内存泄漏，这和引用计数的垃圾回收机制有关，如果两个对象之间形成了循环引用，那么他们都无法被回收，其实这种原因造成的内存泄露本质上并不是闭包造成的，我们可以将这些变量设置为 null，那么他们就会被正常回收

### 高阶函数

> 函数可以作为参数被传递
> 函数可以作为返回值输出

#### AOP(面向切面编程)

他的主要作用是吧一些跟核心逻辑无关的功能抽离出来

#### 高阶函数的应用

1. currying(函数柯里化)，又被称作部分求值。
2. uncurrying

   ```js
   //
   Function.prototype.uncurrying = function() {
     var self = this;
     return function() {
       var obj = Array.prototype.shift.call(arguments);
       return self.apply(obj, arguments);
     };
   };

   var push = Array.prototype.push.uncurrying();
   (function() {
     push(arguments, 4);
     console.log(arguments); // [1, 2, 3, 4]
   })(1, 2, 3);
   //通过uncurrying把push变成了一个通用的方法
   ```

3. 函数节流
   出发频率过高的场景：

   - window.onresize
   - mousemove
   - 上传进度

   ```js
   var throttle = function(fn, interval) {
     var _self = fn;
     var timer;
     var firstTime = true;

     return function() {
       var args = arguments;
       var _me = this;
       if (firstTime) {
         _self.apply(_me, args);
         return (firstTime = false);
       }
       if (timer) {
         return false;
       }
       timer = setTimeout(() => {
         clearTimeout(timer);
         timer = null;
         _self.apply(_me, args);
       }, interval || 500);
     };
   };
   window.onresize = throttle(function() {
     console.log(1);
   }, 500);
   ```

4. 分时函数
   比如把 1 秒创建 1000 个节点，该没每隔 200ms 创建 8 个节点，可以使用 setInterval 实现
   （见 p55）
5. 惰性加载函数

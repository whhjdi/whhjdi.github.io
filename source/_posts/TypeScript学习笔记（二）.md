---
title: TypeScript学习笔记（二）
date: 2018-10-02 21:50:20
tags: [TypeScript]
categories: ["Typescript"]
cover_img: https://ws4.sinaimg.cn/large/006tNbRwly1fxze3mpszyj30xc0dwq3z.jpg
---

# TypeScript

> 最新整理的笔记放到 oneNote 了

接口类似于低配版的类，类就是高配版的接口

## 接口

接口就是描述一个对象必须有什么属性

```typescript
interface Human {
  name: string;
  age: number;
  run(a:number):void
}
let obj: Human = { name: "muxue", age: 18, run:function(18){}};
```

<!--more-->

### 只读属性

```typescript
interface Human {
  readonly name: string;
  readonly age: number;
}
let obj: Human = {};
```

### 可选属性

```typescript
interface Human {
  name?: string;
  age: number;
}
```

### 接口的继承

```typescript
interface Animal {
  move(): void;
}
interface Human extends Animal {
  name: string;
  age: number;
}
let aa: Human = {
  name: "da",
  age: 12,
  move() {},
};
```

## 类

类的属性有 `public` `private` `protected`

```typescript
class Animal {
  constructor() {}
  move(): void {
    console.log("move");
  }
}
class Human extends Animal {
  private name: string;
  protected age: number;
  constructor(name: string = "asd", age: number = 18) {
    super();
    this.name = name;
    this.age = age;
  }
  say(): void {}
}

let aa = new Human();
console.log();
```

### 访问器

```typescript
class Human {
  name: string;
  _age: number;
  get age() {
    return this._age;
  }
  set age(value: number) {
    this._age = value;
  }
}
```

### 抽象类

类似于没有写完的类，只能被继承，不能被实例化

```typescript
abstract class Animal {
  abstract makeSound(): void;
  move(): void {
    console.log("roaming the earch...");
  }
}
```

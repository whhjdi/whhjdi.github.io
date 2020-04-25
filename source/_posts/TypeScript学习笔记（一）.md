---
title: TypeScript学习笔记（一）
date: 2018-09-30 12:54:45
tags: [TypeScript]
categories: ["Typescript"]
cover_img: https://ws4.sinaimg.cn/large/006tNbRwly1fxze3mpszyj30xc0dwq3z.jpg
---

# TypeScript

讲真除了几个新加的数据类型，学起来就感觉和学 es6 一样。。。。。。

## 安装与调试

```typescript
//安装 ts 和 ts-node
npm install -D ts-node
npm install -D typescript
```

<!--more-->

```json
// 创建 tsdemo/.vscode/launch.json 文件
{
  "configurations": [
    {
      "name": "ts-node",
      "type": "node",
      "request": "launch",
      "program": "/user/local/bin/ts-node",
      "args": ["${relativeFile}"],
      "cwd": "${workspaceRoot}",
      "protocol": "inspector"
    }
  ]
  //然后就可以使用 vscode 的调试工具调试了
}
```

```bash
# 当然也可以不用调试工具
# 在首行写下边的命令，给文件添加执行权限
#!/usr/bin/env ts-node
chmod +x xxx.ts
```

## 数据类型

`Number` + `String` + `Array` + `Boolean`+ `enum` + `Tuple` + `void` + `any` + `never` + `null` + `undefined`

## 类型断言

```typescript
(<string>someValue).length;
(someValue as string).length;
//使用 jsx 时只支持 as
```

## 类型转换

```typescript
let a1: number = 123;
let b1: string = a.toString();

let a2: string = "123";
let b2: number = parseFloat(a2);

let a3: number = 123;
let b3: boolean = Boolean(a3);

let obj1 = { name: "muxue", age: 18 };
let string = JSON.stringify(obj);
let obj2 = JSON.parse(string);
```

## 变量声明

使用 `let` 和 `const` 代替 `var`

## 解构赋值和展开运算符

```typescript
{
  let obj = {
    name: "muxue",
    age: 18,
    nation: "China"
  };
  //解构
  let { name, age, nation } = obj;
  console.log(name, age, nation);
}

{
  let arr = ["apple", "orange", "pear"];
  let [fruit1, fruit2, fruit3] = arr;
  console.log(fruit1, fruit2, fruit3);
}

{
  function sayHi({ name, age }: any) {
    console.log(`Hi, ${name}, ${age}`);
  }

  sayHi({ name: "muxue", age: 18 });
}
```


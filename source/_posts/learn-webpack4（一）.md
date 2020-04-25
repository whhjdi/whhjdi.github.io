---
title: learn-webpack4（一）
date: 2017-10-10 11:29:12
tags: [webpack]
categories: ["wbepack"]
cover_img: https://ws3.sinaimg.cn/large/006tNbRwly1fxzdlh2mt3j31fo0u0aer.jpg
---

webpack 作为目前最主流的打包工具，可以极大地方便开发者实现代码加密和多平台的兼容，位前端工程化提供了极好的支持。

## 准备工作

安装

```javascript
npm init -y
npm install --save-dev webpack
npm install --save-dev webpack-cli
```

## 如何打包 JS

首先需要了解 webpack 支持三种模块化的写法,包括 ES6,CommonJS,AMD
先创建一个文件夹 vendor，分别用上边三种写法写三个 js 文件（sum.js,minus.js,multi.js）,代码如下

<!--more-->

```javascript
//sum.js(ES6)
export default function(a, b) {
  return a + b;
}
//multi.js(AMD)
define(function(require, factory) {
  "use strict";
  return function(a, b) {
    return a * b;
  };
});
//minus.js(CommonJS)
module.exports = function(a, b) {
  return a - b;
};
```

然后我们在入口文件（app.js）中引入他们

```javascript
//ES6
import sum from "./vendor/sum.js";
console.log(sum(1, 2));
//CommonJS
var minus = require("./vendor/minus.js");
console.log(minus(2, 1));
//AMD
require(["./vendor/multi.js"], function(multi) {
  console.log(multi(1, 2));
});
```

### 配置文件

webpack 的配置文件是 `webpack.config.js`

```javascript
const path=require("path)
module.exports = {
  entry:{
    app:"./app.js"
  },
  output:{
    publicPath: __dirname + "/dist/",//js引用路径
    path:path.reslove(__dirname."dist"),//打包输出目录
    filename:"bundle.js"
  }
}
```

打包好的 js 文件会放到配置的 dist 目录中，html 中引入打包好的文件就 ok 了，似不似很简单

## 使用 babel 编译 ES6

babel-loader: 负责 es6 语法转化
babel-preset-env: 包含 es6、7 等版本的语法转化规则
babel-polyfill: es6 内置方法和函数转化垫片
babel-plugin-transform-runtime: 避免 polyfill 污染全局变量

### 安装

```javascript
npm install --save-dev babel-loader babel-core babel-preset-env
npm install --save babel-polyfill
npm install --save babel-plugin-transform-runtime
npm install --save babel-runtime
```

注意这里是有坑的，坑死我了，babel-loader 版本可能不匹配，如果遇到问题，用我的 package.json

### babel 配置

```javascript
//.babelrc
{
  "presets": [
    [
      "env",
      {
        "targets": {
          "browsers": ["last 2 versions"]
        }
      }
    ]
  ],
  "plugins": ["transform-runtime"]
}
```

### webpack 配置

```javascript
const path = require("path");
module.exports = {
  entry: {
    app: "./app.js"
  },
  output: {
    filename: "bundle.js"
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /(node_modules)/,
        use: {
          loader: "babel-loader" // 转化需要的loader
        }
      }
    ]
  }
};
```

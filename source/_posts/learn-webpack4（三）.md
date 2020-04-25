---
title: learn-webpack4（三）
date: 2017-10-11 13:56:07
tags: [webpack]
categories: ["wbepack"]
cover_img: https://ws3.sinaimg.cn/large/006tNbRwly1fxzdlh2mt3j31fo0u0aer.jpg
---

## 单页面解决方案

### 代码分割和懒加载

通过 webpack 的写法和内置函数实现

#### webpack 配置

package.json 和上个 demo 一样
webpack 配置如下

<!--more-->

```javascript
const webpack = require("webpack");
const path = require("path");

module.exports = {
  entry: {
    page: "./src/page.js" //
  },
  output: {
    publicPath: __dirname + "/dist/",
    path: path.resolve(__dirname, "dist"),
    filename: "[name].bundle.js",
    chunkFilename: "[name].chunk.js"
  }
};
```

#### page.js 的写法

import 会自动运行引入的代码

```javascript
import(/* webpackChunkName: 'subPageA'*/ "./subPageA").then(function(subPageA) {
  console.log(subPageA);
});

import(/* webpackChunkName: 'subPageB'*/ "./subPageB").then(function(subPageB) {
  console.log(subPageB);
});

import(/* webpackChunkName: 'lodash'*/ "lodash").then(function(_) {
  console.log(_.join(["1", "2"]));
});
export default "page";
```

require.ensure()不会自动执行 js 代码

```javascript
require.ensure(
  ["./subPageA.js", "./subPageB.js"], // js文件或者模块名称
  function() {
    var subPageA = require("./subPageA"); // 引入后需要手动执行，控制台才会打印
    var subPageB = require("./subPageB");
  },
  "subPage" // chunkName
);

require.ensure(
  ["lodash"],
  function() {
    var _ = require("lodash");
    _.join(["1", "2"]);
  },
  "vendor"
);

export default "page";
```

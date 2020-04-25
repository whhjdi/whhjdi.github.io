---
title: learn-webpack4（二）
date: 2017-10-10 12:44:06
tags: [webpack]
categories: ["wbepack"]
cover_img: https://ws3.sinaimg.cn/large/006tNbRwly1fxzdlh2mt3j31fo0u0aer.jpg 
---

# 多页面解决方案

对于 webpack4 提取公共代码需要用到 optimization.splitChunks 配置

## 提取公共代码

pageA 和 pageB 俩个入口文件，都引入了 subPageA,subPageB 和 lodash。
俩个 sub 文件又都引入了 module.js
具体代码看 demo

<!--more-->

## webpack 配置

```json
{
  "name": "webpack4",
  "version": "1.0.0",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "webpack --config webpack.config.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^4.15.1",
    "webpack-cli": "^3.1.2"
  },
  "dependencies": {
    "lodash": "^4.17.11"
  },
  "description": ""
}
```

```javascript
const webpack = require("webpack");
const path = require("path");

module.exports = {
  // 多页面应用
  entry: {
    pageA: "./src/pageA.js",
    pageB: "./src/pageB.js"
  },
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "[name].bundle.js",
    chunkFilename: "[name].chunk.js"
  },
  optimization: {
    splitChunks: {
      cacheGroups: {
        // 注意: priority属性设置，使第三方库先被打包提取
        // 其次: 打包业务中公共代码
        common: {
          name: "common",
          chunks: "all",
          minSize: 1,
          priority: 0
        },
        // 首先: 打包node_modules中的文件
        vendor: {
          name: "vendor",
          test: /[\\/]node_modules[\\/]/,
          chunks: "all",
          priority: 10
        }
      }
    }
  }
};
```

npm start 打包之后，在 dist 目录生成了 4 个文件，然后在 html 中引入

```javascript
  <script src="./dist/common.chunk.js"></script>
  <script src="./dist/vendor.chunk.js"></script>
  <script src="./dist/pageA.bundle.js"></script>
  <script src="./dist/pageB.bundle.js"></script>
```

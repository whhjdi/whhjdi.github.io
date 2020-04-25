---
title: learn-webpack4（十）
date: 2017-10-19 17:38:50
tags: [webpack]
categories: ["wbepack"]
cover_img: https://ws3.sinaimg.cn/large/006tNbRwly1fxzdlh2mt3j31fo0u0aer.jpg 
---

# 自动清理 dist 目录（clean plugin）&& watch mode

## webpack 配置

把第一个例子搬过来用.
注意 webpack 是倒叙的，所以把 CleanWebpackPlugin，放到最后

```javascript
const webpack = require("webpack");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const CleanWebpackPlugin = require("clean-webpack-plugin");

const path = require("path");

module.exports = {
  entry: {
    app: "./app.js"
  },
  output: {
    publicPath: __dirname + "/dist/", // js引用路径或者CDN地址
    path: path.resolve(__dirname, "dist"), // 打包文件的输出目录
    filename: "[name]-[hash:5].bundle.js",
    chunkFilename: "[name]-[hash:5].chunk.js"
  },
  plugins: [
    new HtmlWebpackPlugin({
      filename: "12.html",
      template: "./12.html",
      chunks: ["app"]
    }),
    new CleanWebpackPlugin(["dist"])
  ]
};
```

<!--more-->

### watch mode

```bash
npx webpack --watch
<!-- 监听到变化就会重新打包 -->
npx webpack -w --progress --display-reasons --color
<!-- 显示详细的打包过程 -->
```

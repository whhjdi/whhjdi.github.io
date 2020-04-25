---
title: learn-webpack4（五）
date: 2017-10-14 11:20:30
tags: [webpack]
categories: ["wbepack"]
cover_img: https://ws3.sinaimg.cn/large/006tNbRwly1fxzdlh2mt3j31fo0u0aer.jpg 
---

# SCSS 提取和懒加载

## 使用 ExtractTextPlugin

基本的东西和上个 scss 的 demo 一样
不同的是需要用到使用 ExtractTextPlugin

<!--more-->

```javascript
const path = require("path");
const ExtractTextPlugin = require("extract-text-webpack-plugin");

module.exports = {
  entry: {
    app: "./src/app.js"
  },
  output: {
    publicPath: __dirname + "/dist/",
    path: path.resolve(__dirname, "dist"),
    filename: "[name].bundle.js",
    chunkFilename: "[name].chunk.js"
  },
  module: {
    rules: [
      {
        test: /\.scss$/,
        use: ExtractTextPlugin.extract({
          // 注意 1 不提取为单独样式的使用loader
          fallback: {
            loader: "style-loader"
          },
          use: [
            {
              loader: "css-loader",
              options: {
                minimize: true
              }
            },
            {
              loader: "sass-loader"
            }
          ]
        })
      }
    ]
  },
  plugins: [
    new ExtractTextPlugin({
      filename: "[name].min.css",
      allChunks: false // 注意 2 必须为fasle，否则会包含异步的css
    })
  ]
};
```

### scss 引用和懒加载

在项目入口文件 app.js 中，针对 scss 懒加载，需要引入以下配置代码：

```javascript
import "style-loader/lib/addStyles";
import "css-loader/lib/css-base";
```

剩下我们先设置背景色为红色，在用户点击鼠标后，懒加载 common.scss，使背景色变为绿色。剩余代码如下：

```javascript
import "./scss/base.scss";

var loaded = false;
window.addEventListener("click", function() {
  if (!loaded) {
    //懒加载
    import(/* webpackChunkName: 'style'*/ "./scss/common.scss").then(_ => {
      // chunk-name : style
      console.log("Change bg-color of html");
      loaded = true;
    });
  }
});
```

我们需要在 html 代码中引入打包结果中的、非懒加载的样式(/dist/app.min.css)和 js 文件(/dist/app.bundle.js)。

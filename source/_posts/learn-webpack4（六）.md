---
title: learn-webpack4（六）
date: 2017-10-16 11:37:35
tags: [webpack]
categories: ["wbepack"]
cover_img: https://ws3.sinaimg.cn/large/006tNbRwly1fxzdlh2mt3j31fo0u0aer.jpg 
---

# Tree Shaking

把没有用到的代码在打包的时候丢弃掉，依赖于 ed6 的模块系统

只需要配置 mode 为"production"，即可显式激活 UglifyjsWebpackPlugin 插件。现在默认就是激活的，不配置 mode 也可以

<!--more-->

## JS Tree Shaking

不需要安装其他依赖，package，json 都不需要配置

```javascript
const path = require("path");

module.exports = {
  entry: {
    app: "./src/app.js"
  },
  output: {
    publicPath: __dirname + "/dist/",
    path: path.resolve(__dirname, "dist"),
    filename: "[name].bundle.js"
  },
  mode: "production"
};
```

打包之后会发现 app.bundle.js 只有函数 a，其他没有用的函数都被丢弃了

## 第三方库的 Tree Shaking

由于它是依赖 es6 的模块系统，所以使用的第三方库要使用对应的模块系统版本
比如 lodash 不是 es6 的写法，就需要安装 lodash-es

就这样 啦啦啦

## CSS Tree Shaking

实现 css 的 tree shaking 需要用到 purifycss 和 glod-all
配置一下 webpack

```javascript
const path = require("path");
const PurifyCSS = require("purifycss-webpack");
const glob = require("glob-all");
const ExtractTextPlugin = require("extract-text-webpack-plugin");

let extractTextPlugin = new ExtractTextPlugin({
  filename: "[name].min.css",
  allChunks: false
});

let purifyCSS = new PurifyCSS({
  paths: glob.sync([
    // 要做CSS Tree Shaking的路径文件
    path.resolve(__dirname, "./*.html"), // 请注意，我们同样需要对 html 文件进行 tree shaking
    path.resolve(__dirname, "./src/*.js")
  ])
});

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
        test: /\.css$/,
        use: ExtractTextPlugin.extract({
          fallback: {
            loader: "style-loader",
            options: {
              singleton: true
            }
          },
          use: {
            loader: "css-loader",
            options: {
              minimize: true
            }
          }
        })
      }
    ]
  },
  plugins: [extractTextPlugin, purifyCSS]
};
```

具体例子就不写了，偷个懒

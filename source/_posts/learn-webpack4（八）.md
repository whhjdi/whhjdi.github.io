---
title: learn-webpack4（八）
date: 2017-10-17 16:47:52
tags: [webpack]
categories: ["wbepack"]
cover_img: https://ws3.sinaimg.cn/large/006tNbRwly1fxzdlh2mt3j31fo0u0aer.jpg 
---

# 字体文件处理

## 安装

```json
{
  "devDependencies": {
    "css-loader": "^1.0.0",
    "extract-text-webpack-plugin": "^4.0.0-beta.0",
    "file-loader": "^1.1.11",
    "style-loader": "^0.21.0",
    "url-loader": "^1.0.1",
    "webpack": "^4.16.1"
  }
}
```

需要提取 css 到一个单独的文件，所以需要 ExtractTextPlugin
在 app.js 需要引入 style-loader 和 css-loader

<!--more-->

```javascript
import "style-loader/lib/addStyles";
import "css-loader/lib/css-base";

import "./assets/fonts/iconfont.css";
```

## webpack 配置

```javascript
const path = require("path");
const ExtractTextPlugin = require("extract-text-webpack-plugin");

let extractTextPlugin = new ExtractTextPlugin({
  filename: "[name].min.css",
  allChunks: false
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
            loader: "style-loader"
          },
          use: [
            {
              loader: "css-loader"
            }
          ]
        })
      },
      {
        test: /\.(eot|woff2?|ttf|svg)$/,
        use: [
          {
            loader: "url-loader",
            options: {
              name: "[name]-[hash:5].min.[ext]",
              limit: 5000, // fonts file size <= 5KB, use 'base64'; else, output svg file
              publicPath: "fonts/",
              outputPath: "fonts/"
            }
          }
        ]
      }
    ]
  },
  plugins: [extractTextPlugin]
};
```

打包后会按照你的配置在、dist/fonts/目录下生成打包好的文件

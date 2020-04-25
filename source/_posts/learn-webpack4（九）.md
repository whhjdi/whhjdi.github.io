---
title: learn-webpack4（九）
date: 2017-10-18 17:11:13
tags: [webpack]
categories: ["wbepack"]
cover_img: https://ws3.sinaimg.cn/large/006tNbRwly1fxzdlh2mt3j31fo0u0aer.jpg
---

## 处理第三方 JS 库&&自动生成 html

### 第三方库的管理

1.CDN：`<script></script>`标签引入即可
2.npm 包管理： 目前最常用和最推荐的方法 3.本地 js 文件：一些库由于历史原因，没有提供 es6 版本，需要手动下载，放入项目目录中，再手动引入。
针对第三种方法，如果没有 webpack，则需要手动引入 import 或者 require 来加载文件；但是，webpack 提供了 alias 的配置，配合 webpack.ProvidePlugin 这款插件，可以跳过手动入，直接使用！

<!--more-->

#### webpack 配置

    ```javascript
    // webpack.config.js
    const path = require("path");
    const webpack = require("webpack");

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
      resolve: {
        alias: {
          jQuery$: path.resolve(__dirname, "src/vendor/jquery.min.js")
        }
      },
      plugins: [
        new webpack.ProvidePlugin({
          $: "jquery", // npm
          jQuery: "jQuery" // 本地Js文件
        })
      ]
    };
    ```

    在 app.js 分别使用\$和 jQuery 添加一个 class,webpack 处理后，class 会被正确的添加

### 自动生成 html

    之前我们打包之后都需要手动引入打包好的文件路径，但是实际工作中，这些操作都是自动完成的，那么应该怎么做呢
    借助 HtmlWebpackPlugin 根据指定的 index.html 模板生成对应的 html 文件，还需要配合 html-loader 处理 html 文件中的 `<img>`标签和属性。

    app.js 引入 sum.js

    ```javascript
    //sum.js
    export function sum(a, b) {
      return a + b;
    }
    //app.js
    import sum from "./vendor.sum.js";
    console.log("1 + 2 =", sum(1, 2));
    ```

#### webpack 配置

    HtmlWebpackPlugin 是在 plugin 这个选项中配置的。常用参数含义如下：

    filename：打包后的 html 文件名称
    template：模板文件（例子源码中根目录下的 index.html）
    chunks：和 entry 配置中相匹配，支持多页面、多入口
    minify.collapseWhitespace：压缩选项

    ```javascript
    const path = require("path");
    const webpack = require("webpack");
    const HtmlWebpackPlugin = require("html-webpack-plugin");

    module.exports = {
      entry: {
        app: "./src/app.js"
      },
      output: {
        publicPath: __dirname + "/dist/",
        path: path.resolve(__dirname, "dist"),
        filename: "[name]-[hash:5].bundle.js",
        chunkFilename: "[name]-[hash:5].chunk.js"
      },
      module: {
        rules: [
          {
            test: /\.html$/,
            use: [
              {
                loader: "html-loader"
              }
            ]
          }
        ]
      },
      plugins: [
        new HtmlWebpackPlugin({
          filename: "11.html",
          template: "./11.html",
          chunks: ["app"], // entry中的app入口才会被打包
          minify: {
            // 压缩选项
            collapseWhitespace: true
          }
        })
      ]
    };
    ```

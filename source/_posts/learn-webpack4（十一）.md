---
title: learn-webpack4（十一）
date: 2017-10-22 19:23:03
tags: [webpack]
categories: ["wbepack"]
cover_img: https://ws3.sinaimg.cn/large/006tNbRwly1fxzdlh2mt3j31fo0u0aer.jpg 
---

# 开发模式&&webpack-dev-server

## 开发模式

默认情况下，不指定 mode，打包的时候就默认为 production,开发模式就是我们指定 mode 为 development.开启调试就是 devtool 设置为 source-map,非开发模式关闭此选项以减少体积
在开发模式下，还需要热重载、路由重定向、挂代理等功能，webpack4 已经提供了 devServer 选项，启动一个本地服务器，让开发者使用这些功能。

## 安装

```javascript
 "scripts": {
    "dev": "webpack-dev-server --open"
  },
  "devDependencies": {
    "clean-webpack-plugin": "^0.1.19",
    "html-webpack-plugin": "^3.2.0",
    "jquery": "^3.3.1",
    "webpack": "^4.16.1",
    "webpack-cli": "^3.1.0",
    "webpack-dev-server": "^3.1.4"
  }
```

<!--more-->

安装完之后，运行 npm run dev 就可以启动开发模式,开发模式打包的内容暂时储存在内存中

## wbepack 配置

```javascript
const webpack = require("webpack");
const HtmlWebpackPlugin = require("html-webpack-plugin");

const path = require("path");

module.exports = {
  entry: {
    app: "./app.js"
  },
  output: {
    publicPath: "/",
    path: path.resolve(__dirname, "dist"),
    filename: "[name]-[hash:5].bundle.js",
    chunkFilename: "[name]-[hash:5].chunk.js"
  },
  mode: "development", // 开发模式
  devtool: "source-map", // 开启调试
  devServer: {
    contentBase: path.join(__dirname, "dist"),
    port: 8000, // 本地服务器端口号
    hot: true, // 热重载
    overlay: true, // 如果代码出错，会在浏览器页面弹出“浮动层”。类似于 vue-cli 等脚手架
    proxy: {
      // 跨域代理转发
      "/comments": {
        target: "https://m.weibo.cn",
        changeOrigin: true,
        logLevel: "debug",
        headers: {
          Cookie: ""
        }
      }
    },
    historyApiFallback: {
      // HTML5 history模式
      rewrites: [{ from: /.*/, to: "/index.html" }]
    }
  },
  plugins: [
    new HtmlWebpackPlugin({
      filename: "index.html",
      template: "./index.html",
      chunks: ["app"]
    }),
    new webpack.HotModuleReplacementPlugin(),
    new webpack.NamedModulesPlugin(),
    new webpack.ProvidePlugin({
      $: "jquery"
    })
  ]
};
```

## 模块热更新

模块热更新需要 HotModuleReplacementPlugin 和 NamedModulesPlugin 这两个插件，并且顺序不能错。并且指定 devServer.hot 为 true。

```javascript
if (module.hot) {
  // 检测是否有模块热更新
  module.hot.accept("./vendor/sum.js", function() {
    // 针对被更新的模块, 进行进一步操作
    console.log("/vendor/sum.js is changed");
  });
}
```

## 跨域代理

通过 devServer.proxy 做一个代理转发，来绕过浏览器的跨域限制。
按照前面的配置文件，如果想调用微博的一个接口：https://m.weibo.cn/comments/hotflow。只需要在代码中对/comments/hotflow进行请求即可：

```javascript
$.get(
  "/comments/hotflow",
  {
    id: "4263554020904293",
    mid: "4263554020904293",
    max_id_type: "0"
  },
  function(data) {
    console.log(data);
  }
);
```

## history 模式

在 vuejs 官方的脚手架 vue-cli 中，开发模式下配置如下：

```javascript
historyApiFallback: {
  rewrites: [{ from: /.*/, to: "/index.html" }];
}
```

## 入口文件

```javascript
import sum from "./vendor/sum";
console.log("sum(1, 2) = ", sum(1, 2));
var minus = require("./vendor/minus");
console.log("minus(1, 2) = ", minus(1, 2));
require(["./vendor/multi"], function(multi) {
  console.log("multi(1, 2) = ", multi(1, 2));
});

$.get(
  "/comments/hotflow",
  {
    id: "4263554020904293",
    mid: "4263554020904293",
    max_id_type: "0"
  },
  function(data) {
    console.log(data);
  }
);

if (module.hot) {
  // 检测是否有模块热更新
  module.hot.accept("./vendor/sum.js", function() {
    // 针对被更新的模块, 进行进一步操作
    console.log("/vendor/sum.js is changed");
  });
}
```

npm run dev 就可以看到结果了，由于开启了 source-map，所以可以定位代码位置。由于开起了热重载，当我们改变了 sum.js 就会自动执行编译

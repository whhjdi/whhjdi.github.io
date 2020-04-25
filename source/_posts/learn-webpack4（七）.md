---
title: learn-webpack4（七）
date: 2017-10-16 12:19:10
tags: [webpack]
categories: ["wbepack"]
cover_img: https://ws3.sinaimg.cn/large/006tNbRwly1fxzdlh2mt3j31fo0u0aer.jpg
---

## 图片处理

图片处理 和 Base64 编码 (url-loader)
图片压缩 (img-loader)
合成雪碧图 (postcss-loader,postcss-sprites)

<!--more-->

### base64 编码和图片压缩

```javascript
// webpack.config.js

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
        test: /\.(png|jpg|jpeg|gif)$/,
        //图片压缩
        use: [
          {
            loader: "url-loader",
            options: {
              name: "[name]-[hash:5].min.[ext]",
              limit: 1000, // size <= 1KB
              publicPath: "static/",
              outputPath: "static/"
            }
          },
          // img-loader for zip img
          {
            loader: "img-loader",
            options: {
              plugins: [
                require("imagemin-pngquant")({
                  quality: "80" // the quality of zip
                })
              ]
            }
          }
        ]
        //小于20KB的图片转为base64格式
        // use: [
        //   {
        //     loader: "url-loader",
        //     options: {
        //       name: "[name]-[hash:5].min.[ext]",
        //       limit: 20000, // size <= 20KB
        //       publicPath: "static/",
        //       outputPath: "static/"
        //     }
        //   }
        // ]
      }
    ]
  },
  plugins: [extractTextPlugin]
};
```

### 雪碧图合成

根据文档加入相应的配置即可

```javascript
const path = require("path");
const ExtractTextPlugin = require("extract-text-webpack-plugin");

let extractTextPlugin = new ExtractTextPlugin({
  filename: "[name].min.css",
  allChunks: false
});

/*********** sprites config ***************/
let spritesConfig = {
  spritePath: "./dist/static"
};
/******************************************/

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
            },
            /*********** loader for sprites ***************/
            {
              loader: "postcss-loader",
              options: {
                ident: "postcss",
                plugins: [require("postcss-sprites")(spritesConfig)]
              }
            }
            /*********************************************/
          ]
        })
      },
      {
        test: /\.(png|jpg|jpeg|gif)$/,
        use: [
          {
            loader: "url-loader",
            options: {
              name: "[name]-[hash:5].min.[ext]",
              limit: 10000, // size <= 20KB
              publicPath: "static/",
              outputPath: "static/"
            }
          },
          {
            loader: "img-loader",
            options: {
              plugins: [
                require("imagemin-pngquant")({
                  quality: "80"
                })
              ]
            }
          }
        ]
      }
    ]
  },
  plugins: [extractTextPlugin]
};
```

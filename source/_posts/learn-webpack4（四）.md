---
title: learn-webpack4（四）
date: 2017-10-12 16:19:21
tags: [webpack]
categories: ["wbepack"]
cover_img: https://ws3.sinaimg.cn/large/006tNbRwly1fxzdlh2mt3j31fo0u0aer.jpg 
---

# 打包 css/scss

通过 webpack 可以有以下几种方式
1，将 css 通过 link 标签引入
2，将 css 放到 style 标签里
3，动态卸载和加载 css
4, 先加载 css.transform

## 打包 css

### webpack 配置

```javascript
{
  "devDependencies": {
    "css-loader": "^1.0.0",
    "file-loader": "^1.1.11",
    "style-loader": "^0.21.0"
  }
}
```

<!--more-->

4 种配置方案，只有 use 部分不同，我就写一起了

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
  module: {
    rules: [
      {
        test: /\.css$/, // 针对CSS结尾的文件设置LOADER
        //transform
        use: [
          {
            loader: "style-loader",
            options: {
              transform: "./css.transform.js" // transform 文件
            }
          },
          {
            loader: "css-loader"
          }
        ]
        // use,unuse
        // use: [
        //   {
        //     loader: "style-loader/useable" // 注意此处的style-loader后面的 useable
        //   },
        //   {
        //     loader: "css-loader"
        //   }
        // ]

        // css放在style里
        // use: [
        //   {
        //     loader: "style-loader",
        //     options: {
        //       singleton: true // 处理为单个style标签
        //     }
        //   },
        //   {
        //     loader: "css-loader",
        //     options: {
        //       minimize: true // css代码压缩
        //     }
        //   }
        // ]

        // 以link的形式引入
        // use: [
        //   {
        //     loader: "style-loader/url"
        //   },
        //   {
        //     loader: "file-loader"
        //   }
        // ]
      }
    ]
  }
};
```

### 打包后以 link 形式引入

只需要 fill- 来处理即可

```javascript
let clicked = false;
window.addEventListener("click", function() {
  // 需要手动点击页面才会引入样式！！！
  if (!clicked) {
    import("./css/base.css");
  }
});
```

app.js 写如上代码，点击即可看到以 link 形式引入了 css

### 打包后将 css 放到 style 里

style.loader 处理为单个 style 标签
css-loader 做代码压缩

###动态卸载和加载 css
通过 style-loader 提供的 use()和 unuse()方法实现
修改 app.js 实现每 0.5s 换一次背景颜色

```javascript
import base from "./css/base.css"; // import cssObj from '...'
var flag = false;
setInterval(function() {
  // unuse和use 是 cssObj上的方法
  if (flag) {
    base.unuse();
  } else {
    base.use();
  }
  flag = !flag;
}, 500);
```

### 加载 css 前更改样式

通过 options.transform 实现
写一个 css.transform.js

```javascript
module.exports = function(css) {
  console.log(css); // 查看css
  return window.innerWidth < 1000 ? css.replace("red", "green") : css; // 如果屏幕宽度 < 1000, 替换背景颜色
};
```

在 app.js 中引入 css 文件

```javascript
import base from "./css/base.css";
```

如果打开网页时宽度<1000,那么背景颜色就是绿色，否则就是灰色
但是并不是响应式的

## 打包 scss

首先需要用到 node-sass,sass-loader 等加载器

```javascript
{
  "devDependencies": {
    "css-loader": "^1.0.0",
    "extract-text-webpack-plugin": "^4.0.0-beta.0",
    "node-sass": "^4.9.2",
    "sass-loader": "^7.0.3",
    "style-loader": "^0.21.0",
    "webpack": "^4.16.0"
  }
}
```

### 思路

首先使用 sass-loader 把 scss 编译成 css,然后再按照用 css 的相关 loader 处理，注意放在最后的 loader 首先被执行

---
title: 天马行空的CSS（二）
date: 2018-03-15 17:27:12
tags: [CSS]
categories: ["css"]
cover_img: https://ws1.sinaimg.cn/large/006tNbRwly1fy1m2svp2qj31hc0u01kx.jpg
---

# CSS

## BFC

满足某种条件的元素会触发 BFC(块级格式化上下文),见 [MDN](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context)
功能举例:
用 BFC 包住浮动元素:
例如: overflow:hidden 清除浮动.
用 BFC 取消父子 margin 合并
例如: overflow:hidden 取消父子 margin 合并[查看 demo](http://jsbin.com/conulod/1/edit?html,css,js,output)
兄弟元素之间划清界限,不相互影响
例如: 用 float 做左右自适应布局 [查看 demo](http://js.jirengu.com/loyobireze/1/edit))

<!--more-->

## 堆叠上下文

满足某种条件的元素组成了堆叠上下文，详情见 [MDN](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Understanding_z_index/The_stacking_context)

### 堆叠顺序

```html
负z-index < bg < border < div/块级元素 < 浮动元素 <
文字/内联元素 < z-index:0/auto < +z-index
后出现的盖住原来的,只有被定位的元素才能用z-index
```

## 图标的做法

### img

可以自适应宽高，图片会默认保持比例。
只需设置一下 width 或者 height 就能等比改变图标大小。
不能同时设置图标宽高，如果同时设置，图标会变形

### background

图标不会因为改变 div 的宽、高改变而变形，但是图标可能不居中

```css
div {
  background: #ccc url(./qq.png) no-repeat 0 0;
}
```

### 雪碧图

把多张图片合成到一张大图中，可以减少 http 请求的数量
[在线生成 1](https://www.toptal.com/developers/css/sprite-generator)
[在线生成 2](http://css.spritegen.com/)

### iconfont

使用阿里的图标库非常方便[链接](http://iconfont.cn/)

## 响应式页面

### meta:vp

```javascript
<meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
```

### 媒体查询

两种方式

```css
@media (max-width: 320px) {
  body {
    background: red;
  }
}
```

```html
<link rel="stylesheet" href="" media="only screen and(max-width:320px;)">
```

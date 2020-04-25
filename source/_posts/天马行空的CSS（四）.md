---
title: 天马行空的CSS（四）
date: 2018-03-18 18:32:27
tags: [CSS]
categories: ["css"]
cover_img: https://ws3.sinaimg.cn/large/006tNbRwly1fy1m2f921fj315o0tnay9.jpg
---

# CSS

## 布局

[常见的布局 demo](https://github.com/whhjdi/bash_demo/tree/master/layout-demo)
[预览链接](http://whhjdi.xyz/bash_demo/layout-demo/index.html)

浮动布局，flex 布局，flex 和负 margin 布局定宽或者使用 calc 不定宽等方式基本能实现大多数的布局，在搭配绝对定位等可以实现几乎所有布局

<!--more-->

## 常见居中方式

[css-tricks](https://css-tricks.com/centering-css-complete-guide/)总结了各种居中的方式可以参考

### 水平居中

```css
/* 内联元素 适用于在块级的父元素中水平放置内联元素   */
.parent {
  text-align: center;
}
/* 块级元素 可以加宽度(width:100px;)或者改变 display(display:table;) .parent { */
  margin: 0 auto;
}
/* 多个块级元素 子元素设置 inline-block，同时父元素设置 text-align:center */
.parent {
  text-align: center;
}
.child {
  display: inline-block;
}
```

### 垂直居中

```css
/* 单行的内联元素 设置上下 padding 相同  */
.child {
  padding: 20px 0;
}
/* 设置行高等于高度  */
.chind {
  line-height: 100px;
  height: 100px;
}
/* 多行的内联元素通过设置父元素table，子元素table-cell和vertical-align
vertical-align:middle的意思是把元素放在父元素的中部 */
.parent {
  display: table;
}
.child {
  display: table-cell;
  vertical-align: middle;
}
/* 已知高度的块级元素 利用 position 和 top 和负 margin, */
.parent {
  position: relative;
  height: 300px;
}
.child {
  position: absolute;
  top: 50%;
  height: 100px;
  margin-top: -50px;
}
/* 利用 position 和 top/bottom 和 margin:auto  */
.parent {
  position: relative;
  height: 300px;
}
.child {
  height: 100px;
  position: absolute;
  top: 0;
  bottom: 0;
  margin: auto;
}
/* 未知高度的块级元素 利用 position 和 top 和 transform  */
.parent {
  position: relative;
}
.child {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
}
```

### 水平垂直居中

```css
/* flex 大法好 */

.parent{
display:flex;
justify-content:center;
align-items:center;
}
/* 综合以上水平或者垂直居中的方法 */

/* 绝对定位+margin:auto */

.child{
width:100px;
height:100px;
position:absolute;
top:0;
bottom:0;
left:0;
right:0;
margin:auto;
}
/* 绝对定位和负 margin */

.child{
width:100px;
height:100px;
position:absolute;
top:50%;
left:50%
margin-left:-50px;
margin-top: -50px;
}
/* 绝对定位和 transform */

.child {
position: absolute;
top: 50%;
left:50%
transform: translate(-50%,-50%);
}
/* table-cell */

.child{
display:table-cell;
text-align:center;
vertical-align: middle;
}
grid 布局暂时不用
```

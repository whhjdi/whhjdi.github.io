---
title: 天马行空的CSS（一）
date: 2018-03-13 12:33:38
tags: [CSS]
categories: ["css"]
cover_img: https://ws2.sinaimg.cn/large/006tNbRwly1fy1m3a4g42j30xc0irnob.jpg
---

# CSS

## 文档流

### 什么是文档流

```html
内联元素从左到右排列
块级元素从上到下排列
div 的高度是由其内部文档流元素的总和决定的
```

### 脱离文档流

```html
浮动- float:left;
position: absolute;
position: fixed;
```

<!--more-->

### 如何清除浮动

```css
.clearfix::after {
  content: "";
  display: block;
  clear: both;
  zoom: 1; /*兼容 IE*/
}
或者overflow: hidden;
```

## 宽度与高度

### 内联元素的宽高

```html
内联元素的高度由行高决定,和 padding 无关
宽度受 padding,margin,content,border 影响
```

### 块级元素的宽高

```html
如果子元素是内联元素,高度是所有内联元素行高的和
如果子元素是块级元素,高度会受 padding,border 影响
不一定受 margin 影响
```

### margin 合并

```html
如果父元素没有东西(border,padding)挡住子元素的
margin,那么子元素的上下 margin 会超出父元素
这个 margin 会和父元素的 margin 合并,父元素就不会变高,
如果父元素有上边框,那么,这个 margin 会使父元素变高
```

阻止 margin 合并

```html
border,padding,overflow:hidden
```

## 文字溢出省略

一行文字：

```css
 {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
```

多行文字：

```css
 {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
```

## 盒模型

```html
content-box: width == 内容区宽度
border-box: width == 内容区宽度 + padding 宽度 + border 宽度
```

### 如何获得宽高

1、dom.style.width/height
这种方式只能取到 dom 元素内联样式所设置的宽高，也就是说如果该节点的样式是在 style 标签中或外联的 CSS 文件中 设置的话，通过这种方法是获取不到 dom 的宽高的

2、dom.currentStyle.width/height
这种方式获取的是在页面渲染完成后的结果，就是说不管是哪种方式设置的样式，都能获取到，只有 IE 浏览器支持该方式

3、window.getComputedStyle(dom).width/height
这种方式的原理和 2 是一样的，这个可以兼容更多的浏览器，通用性好一些

4、 dom.getBoundingClientRect().width/height
这种方式是根据元素在视窗中的绝对位置来获取宽高的

5、 dom.offsetWidth/offsetHeight

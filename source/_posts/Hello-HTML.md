---
title: Hello-HTML
date: 2018-03-03 11:55:57
tags: [HTML]
categories: ["html"]
cover_img: https://ws3.sinaimg.cn/large/006tNbRwly1fxzdw01bysj30u011ib29.jpg
---

## 如何理解 HTML 的语义化

最早的时候是没有前端程序员的,一些后端程序员写 html,但是他们中的一些人对 css 不太了解,就用 table 布局,但是,table 是用来展示表格的,这就违反了语义化
之后出现了专门写 css 的前端,他们用 div+css 布局,主要用 float 和绝对定位布局,稍微符合了 html 的语义化
再后来,有了专门的前端程序员,他们清楚的知道每个标签的用法,不再是全用 div 标签了,会尽量使用 h1, ul, ol, p, header,nav 等
语义化可以让页面具有良好的结构与含义,可以有效的提高代码可读性和可检索性

<!--more-->

## HTML5 的新特性

新的语义化元素：article 、footer 、header 、nav 、section
表单增强，新的表单控件：calendar 、date 、time 、email 、url 、search
新的 API：音频(用于媒介回放的 video 和 audio 元素)、图形（绘图 canvas 元素）
新的 API：离线，通过创建 cache manifest 文件，创建应用程序缓存
新的 API：本地存储，localStorage-没有时间限制的数据存储，sessionStorage-session 数据存储（关闭浏览器窗口数据删除）
新的 API：实时通讯，设备能力

## meta viewport

```javascript
<meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
```

早些时候的页面都是给 PC 准备的，后来智能手机的出现，导致页面不适应手机屏幕，苹果的工程师就想了个办法，把手机模拟成 980px,使页面完整的展示，但是所有的东西都缩小了。后来，智能手机普及，有了专门适配手机的页面，也就不需要再缩小页面了

## canvas

Canvas 元素(HTML5 新增)可用于通过使用 JavaScript 中的脚本来绘制图形。例如，它可以用于绘制图形，制作照片，创建动画，甚至可以进行实时视频处理或渲染。

```javascript
var canvas = document.getElementById("tutorial");
var ctx = canvas.getContext("2d");
```

其实基本的用法比较简单，我们可以非常方便的画一些简单的图形（矩形，圆形等）

举个栗子
绘制矩形我们有三种方式

```javascript
//1
ctx.fillRect(x, y, width, height);
//2
ctx.beginPath();
ctx.moveTo(x, y);
ctx.lineTo(x + width, y);
ctx.lineTo(x + width, y + height);
ctx.lineTo(x, y + height);
ctx.lineTo(x, y);
ctx.fill();
//3
ctx.rect(x, y, width, height);
ctx.fill();
```

我使用 canvas 制作了一个画板，有兴趣的可以看一看[点击查看](http://whhjdi.xyz/canvas-demo/)

## SVG

1、SVG 可缩放矢量图形（Scalable Vector Graphics），是一种使用可扩展标记语言（XML）描述 2D 图形的语言
2、不依赖分辨率,逐元素（DOM 元素）进行渲染,能够很好的处理图形大小的改变, 放大或缩小时图形质量不会有所损失
3、 基于 XML，用文本格式的描述性语言来描述图像内容
4、 支持事件处理器。SVG 绘制出的每个图形元素都是独立的 DOM 节点，能够方便的绑定事件
5、 适合静态图片展示，高保真文档查看和打印的应用场景，不适合游戏应用）
6、 如果对象属性发生变化，浏览器能自动重现图形。也就是说，SVG 绘图很容易编辑，只需要增加或移除相应的元素即可

## 浏览器内核

Trident 内核(IE 内核)
Gecko(Firefox 内核)
Webkit 内核(Safari 内核,Chrome 内核原型,开源)
Presto 内核
Blink 内核

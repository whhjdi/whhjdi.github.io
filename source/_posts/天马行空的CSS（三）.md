---
title: 天马行空的CSS（三）
date: 2018-03-15 18:14:27
tags: [CSS]
categories: ["css"]
cover_img: https://ws3.sinaimg.cn/large/006tNbRwly1fy1m31on82j30rs0h7aqx.jpg
---

# CSS

## 动画

css 动画一般分为三种
1、transition 补间动画
2、keyframe 关键帧动画
3、animation 逐帧动画

<!--more-->

### 补间动画

1、位置-平移（left/right/margin/transform）
2、方位-旋转（transform）
3、大小-缩放（transform）
4、透明度（opacity）
5、其他线性变换（transform）

### keyframe 关键帧动画

```css
@keyframes testAnimation {
  0% {
    background: red;
    left: 0;
    top: 0;
  }
  25% {
    background: yellow;
    left: 200px;
    top: 0;
  }
  50% {
    background: blue;
    left: 200px;
    top: 200px;
  }
  75% {
    background: green;
    left: 0;
    top: 200px;
  }
  100% {
    background: red;
    left: 0;
    top: 0;
  }
}
div {
  width: 100px;
  height: 50px;
  position: absolute;

  animation-name: myfirst;
  animation-duration: 5s;
}
```

### animation 逐帧动画

```css
.fadeIn {
  animation: fadeIn 0.5s ease 1s both;
}
@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
```

## rem 和 em

rem(root em):根元素（html）的 font-size
em:相对自身字体大小（一般情况下就是父元素的字体大小）

## 其他

font-size
一款字体会定义一个 em-squre,字体度量就基于这个相对单位设置的

line-height
内联元素真实占用的高度
不同字体的 line-height 可能是不同的

vertical-align:top
元素实际占的高度的顶部对齐

其他
display:none 到 display:block 的过程 transition: all 1s;没效果，
可以改成 visibility: hidden;和 visibility: visible

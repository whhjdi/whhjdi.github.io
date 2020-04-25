---
title: 探索react-生命周期
date: 2019-02-11 13:19:43
tags: [react]
cover_img: https://ws4.sinaimg.cn/large/006tKfTcly1g07695jp6kj30i309it8p.jpg
feature_img:
description:
keywords: [react]
---

# 生命周期

![](https://upload-images.jianshu.io/upload_images/5287253-82f6af8e0cc9012b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)

- 创建时：
  ->constructor(初始化内部状态，唯一可以直接修改 state 的地方)
  ->getDerivedStateFromProps(当 state 需要从 props 初始化时使用，不推荐使用，每次 render 都会调用，场景：表单控件获取默认值)
  ->render
  ->componentDidMount(ui 渲染完成后调用，只执行一次，场景：获取外部资源)

- 更新时：(new props,setState,forceUpdate)
  ->getDerivedStateFromProps
  ->shouldComponentUpdate(决定虚拟 dom 是否需要重绘，用于性能优化)
  ->render
  ->getSnapshotBeforeUpdate(在 render 之前调用，state 已更新，场景：获取 render 之前的 dom)
  ->componentDidUpdate(每次 ui 更新时调用)

- 卸载时：
  ->componentWillUnmount(组件移除时调用)

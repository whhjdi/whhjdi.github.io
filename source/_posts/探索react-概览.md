---
title: 探索react-概览
date: 2019-02-11 12:25:16
tags: [react]
cover_img: https://ws4.sinaimg.cn/large/006tKfTcly1g07695jp6kj30i309it8p.jpg
feature_img:
description:
keywords: [react]
---

# react

传统的 DOM API 过度的关注细节，react 始终整体刷新，摆脱传统的局部更新，让我们从频繁的 dom 操作中解放出来

## 组件

props(外部传进来的) + state(自己本身的) -> view
单一职责原则
组件尽量无状态，通过 props 传过来
能够计算得到的状态就不要单独存储
React 认为大写字母开头的是自定义组件，小写字母开头的是原生 dom 节点

### 高阶组件

接收一个组件，返回一个组件

### 函数作为子组件

## 组件拆分

将业务逻辑拆分成高内聚低耦合的模块

- APP
  - component
    - Feature1
    - Feature2
  - route
    - Feature1
    - Feature2
  - reducer
    - Feature1
    - Feature2

## jsx

```jsx
const name= 'muxue'
const  element = <h1>hello {name}</h1>
//其实是
const name= 'muxue'
const element = React.createElement(
  'h1',//标签名
  null,//属性
  'hello'//children
  name //children
)
```

## 虚拟 dom

O(n)
采取 广度优先的分层比较
从根节点开始比较

## Context Api

provider consume

## 单项数据流

## 单元测试

Jest
Js Dom
Enzyme
nock(模拟请求)
sinon(函数模拟)
istanbul(测试覆盖率)

---
title: 探索react-redux
date: 2019-02-11 14:49:53
tags: [react, redux]
cover_img: https://ws4.sinaimg.cn/large/006tKfTcly1g07695jp6kj30i309it8p.jpg
feature_img:
description:
keywords: [react]
---

# redux

提供唯一一个 store 来存储数据，view 中尽量没有其他状态，使得组件通信更加方便

action 是唯一可以改变状态的，也就是改变状态必须派发一个 action

通过纯函数(reducer)来更新 store(实际上是生成一个新的 store)

## store

```js
const store = createStore(reducer);
```

getState()
dispatch(action)
subscribe(listener)

## action

```javascript
{
  type:'ADD_TODO',
  text:'hello'
}
```

## reducer

```javascript
function todoApp(state = initialState, action) {
  switch(action.type){
    case ADD_TODO:
    //返回一个新的state
     return Object.assign({},state,{
       todos:[
         ..state.todos,
         {
           text:action.text,
           completed: false
         }
       ]
     })
     default:
      return state
  }
}
```

- 常用函数
  combineReducers //把多个 reducer 连接起来
  bindActionCreators //接收 {actionCreator1,actionCreator2} 和 dispatch，返回包装好的函数 ，直接 调用就实现了 dispatch action

## 在 react 从使用 redux

```javascript
import { connect } from "react-redux";
connect(mapStateToProps,mapDispatchToProps)(...)
```

## 中间件

当派发一个异步的 action 时，就需要中间件处理这个异步，然后再派发给 reducer

- 常用的中间件
  redux-thunk,redux-saga

## 不可变数据

只需要比较俩个状态是不是同一个引用就能判断状态是否更新

- Object.asign()或者{...}
- immutability-helper
- immer

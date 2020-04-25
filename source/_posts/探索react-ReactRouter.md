---
title: 探索react-ReactRouter
date: 2019-02-11 16:18:36
tags: [react]
cover_img: https://ws4.sinaimg.cn/large/006tKfTcly1g07695jp6kj30i309it8p.jpg
feature_img:
description:
keywords: [react]
---

# React Router

声明式路由，动态路由

## 三种实现方式

1. url 路由 (BrowserRouter)(react-router-dom 提供)
2. hash 路由 (HashRouter)(react-router-dom 提供)
3. 内存路由 (MemoryRouter)(react-router 提供)

## API

```javascript
1. <Link to='/about'/> 普通连接，不会触发浏览器刷新
2. <NavLink to='/about' activeClassName='selected'/> 有选中状态
3. <Prompt when={formIsHalfFilledOut} message="success"/> 满足条件确认离开
4. <Router exact path='/' render={()=>(
    loggedIn?(<Rediect to="/xxx"></Rediect>
    ): (
    <Hello/>
    )
   )}/> 重定向
5. <Route exact path="/" component={Home}></Route> exact是精确匹配
6. <Switch>
    <Route exact path="/" component={Home}></Route>
    <Route exact path="/a" component={Home1}></Route>
    <Route exact path="/b" component={Home2}></Route>
    </Switch>
  只显示第一个匹配的路由
```

## 参数传递

```javascript
const Topic = ({ match }) =>l <h1>Topic {match.params.id}</h1>;
<Router>
	<Link to="/topic/1">Topic 1</Link>
	<Route path="/topic/:id" component={Topic} />
</Router>;
```

通过 this.props.match.id 获取参数

## 嵌套路由

## 管理路由授权

通过区分公开路由和受保护路由，访问未授权路由是重定向到指定页面

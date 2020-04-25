---
title: 遇见koa
date: 2018-12-16 12:37:59
tags: [koa]
cover_img: https://ws2.sinaimg.cn/large/006tNbRwly1fyk1oqw2yej30u011ie3a.jpg
feature_img:
description:
keywords: [node, koa]
---

# koa2

koa 是 Express 的下一代基于 Node.js 的 web 框架，koa2 基于 ES7 开发，完全的使用 promise 配合 async 实现异步

## 基本结构

```javascript
const koa = require("koa");

const app = new koa();

app.use(async (ctx, next) => {
	await next();
	ctx.response.type = "text/html";
	ctx.response.body = "<h1>hello koa2</h1>";
});
const port = 3000;
app.listen(port, () => {
	console.log(`app started at port ${port}`);
});
```

那么处理不同的路径就可以写`if(){}else{}`或者多个 app.use，就像这样

```javascript
app.use(async (ctx, next) => {
	if (ctx.request.path === "/") {
		ctx.response.body = "index page";
	} else {
		await next();
	}
});

app.use(async (ctx, next) => {
	if (ctx.request.path === "/test") {
		ctx.response.body = "TEST page";
	} else {
		await next();
	}
});
```

这就写法又太啰嗦了，这时候就可以引入中间件来集中处理这些 url

## 中间件

### koa-router

基本的结构

```javascript
const koa = require("koa");
const Router = require("koa-router");
const app = new koa();
const router = new Router();
router
	.get("/", async (ctx, next) => {
		ctx.body = "koa-router";
	})
	.get("/js", async (ctx, next) => {
		ctx.body = "js";
	});

app.use(router.routes()).use(router.allowedMethods());
const port = 3000;
app.listen(port, () => {
	console.log(`app started at port ${port}`);
});
```

路由的层级

```javascript
const koa = require("koa");
const Router = require("koa-router");
const app = new koa();

let home1 = new Router();
home1
	.get("/js1", async (ctx, next) => {
		ctx.body = "js1";
	})
	.get("/js2", async (ctx, next) => {
		ctx.body = "js2";
	});

let home2 = new Router();
home2
	.get("/koa", async (ctx, next) => {
		ctx.body = "koa";
	})
	.get("/koa2", async (ctx, next) => {
		ctx.body = "koa2";
	});

let router = new Router();
router.use("/home1", home1.routes(), home1.allowedMethods());
router.use("/home2", home2.routes(), home2.allowedMethods());
app.use(router.routes()).use(router.allowedMethods());
const port = 3000;
app.listen(port, () => {
	console.log(`app started at port ${port}`);
});
```

### koa-bodyparser

解析原始 request 请求

```javascript
const koa = require("koa");
const app = new koa();
const bodyParse = require("koa-bodyparser");
app.use(bodyParse());
app.use(async (ctx, next) => {
	if (ctx.url === "/" && ctx.method === "GET") {
		let html = `
            <form method="POST" action="/">
                <p>userName</p>
                <input name="userName" /><br/>
                <p>age</p>
                <input name="age" /><br/>
                <p>website</p>
                <input name="webSite" /><br/>
                <button type="submit">submit</button>
            </form>
        `;
		ctx.body = html;
	} else if (ctx.url === "/" && ctx.method === "POST") {
		ctx.body = ctx.request.body;
	} else {
		ctx.body = "404";
		await next();
	}
});
const port = 3000;
app.listen(port, () => {
	console.log(`app started at port ${port}`);
});
```

## 模板引擎

使用 ejs 和 koa-views
在根目录新建 view 文件夹，写一个 hello.ejs 文件

```javascript
const koa = require("koa");
const views = require("koa-views");
const path = require("path");
const app = new koa();

app.use(
	views(path.join(__dirname, "./view"), {
		extension: "ejs"
	})
);

app.use(async (ctx, next) => {
	let title = "hello";
	await ctx.render("hello", {
		title
	});
});
const port = 3000;
app.listen(port, () => {
	console.log(`app started at port ${port}`);
});
```

如果需要使用静态资源则需要引入 koa-static,然后就可以直接用资源名访问

```javascript
const koa = require("koa");
const static = require("koa-static");
const path = require("path");
const app = new koa();

const staticPath = "./static";
app.use(static(path.join(__dirname, staticPath)));
app.use(async ctx => {
	ctx.body = "hello world";
});
const port = 3000;
app.listen(port, () => {
	console.log(`app started at port ${port}`);
});
```

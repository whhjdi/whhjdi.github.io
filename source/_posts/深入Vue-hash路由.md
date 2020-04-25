---
title: 深入Vue-hash路由
date: 2019-04-10 16:27:01
tags:
cover_img: https://i.loli.net/2019/08/21/lc5eEv8MiRLWnKA.jpg
feature_img:
description:
keywords:
---
# hash router

## 5.1 basic hash router

### 要求

- display foo when url is at #foo
- display bar when url is at #bar
- bonus: implement links that navigate between #foo and #bar
- to access the current hash:
  ```js
	window.location.hash
	```
- to listen for hash changes:
  ```js
	window.addEventListener('hashchange', () => {
		//read hash and update app
	})
	```

### 实现
利用window.location.hash就可以实现了，然后监听hashchange事件改变url,来渲染不同的组件
```html
<script src="../node_modules/vue/dist/vue.js"></script>
<div id="app">
  <component :is="url"></component>
  <a @click="routeTo('#foo')" href="#foo">foo</a>
  <a @click="routeTo('#bar')" href="#bar">bar</a>
</div>

<script>
window.addEventListener('hashchange', () => {
  app.url = window.location.hash.slice(1)
})

const app = new Vue({
  el: '#app',
  data: {
    url: 'foo'
  },
  components: {
    foo: { template: `<div>foo</div>`},
    bar: { template: `<div>bar</div>`},
  },
  methods: {
    routeTo (route) {
      window.location.hash = route
    }
  }
})
</script>

```

## 5.2 router table

换一种形式，实现一个路由表，来保存所有的路由，当没有匹配到时，render404

```html
<script src="../node_modules/vue/dist/vue.js"></script>

<div id="app">
  <component :is="matchedComponent"></component>
  <a href="#foo">foo</a>
  <a href="#bar">bar</a>
</div>

<script>
// '#/foo' -> Foo
// '#/bar' -> Bar
// '#/404' -> NotFound

const Foo = { template: `<div>foo</div>` }
const Bar = { template: `<div>bar</div>` }
const NotFound = { template: `<div>not found!</div>` }

const routeTable = {
  foo: Foo,
  bar: Bar
}

window.addEventListener('hashchange', () => {
  app.url = window.location.hash.slice(1)
})

const app = new Vue({
  el: '#app',
  data: {
    url: 'foo'
  },
  computed: {
    matchedComponent () {
      return routeTable[this.url] || NotFound
    }
  }
})
</script>

```

## 路由传参

在vue-router中传参的形式如下

```js
'/user/:username'
'/user/123?foo=bar'
{
	path: '/user/123',
	params: {username:123},
	query: {foo:'bar'}
}
```

那么如何实现一个类似的东西呢
首先需要一个库 `path-to-regexp`

```js
	'#/foo/123' -> foo with id: 123
	'#/bar' -> Bar
	'#/404' -> NotFound

	path-to-regexp usage:
	const regex = pathToRegexp(pattern)
	const match = regex.exec(path)
```

### 实现

```html
<script src="../node_modules/vue/dist/vue.js"></script>
<script src="./path-to-regexp.js"></script>

<div id="app"></div>

<script>

	const Foo = {
		props: ["id"],
		template: `<div>foo with id: {{ id }}</div>`
	};
	const Bar = { template: `<div>bar</div>` };
	const NotFound = { template: `<div>not found!</div>` };

	const routeTable = {
		"/foo/:id": Foo,
		"/bar": Bar
	};

	const compiledRoutes = [];
	Object.keys(routeTable).forEach(path => {
		const dynamicSegments = [];
		const regex = pathToRegexp(path, dynamicSegments);
		console.log(regex);

		const component = routeTable[path];
		compiledRoutes.push({
			component,
			regex,
			dynamicSegments
		});
	});

	window.addEventListener("hashchange", () => {
		app.url = window.location.hash.slice(1);
	});

	const app = new Vue({
		el: "#app",
		data: {
			url: window.location.hash.slice(1)
		},
		render(h) {
			const path = "/" + this.url;

			let componentToRender;
			let props = {};

			compiledRoutes.some(route => {
				const match = route.regex.exec(path);
				console.log(match);

				componentToRender = NotFound;
				if (match) {
					componentToRender = route.component;
					route.dynamicSegments.forEach((segment, index) => {
						props[segment.name] = match[index + 1];
					});
					return true;
				}
			});

			return h("div", [
				h(componentToRender, { props }),
				h("a", { attrs: { href: "#foo/123" } }, "foo 123"),
				" | ",
				h("a", { attrs: { href: "#foo/234" } }, "foo 234"),
				" | ",
				h("a", { attrs: { href: "#bar" } }, "bar"),
				" | ",
				h("a", { attrs: { href: "#garbage" } }, "garbage")
			]);
		}
	});
</script>

```
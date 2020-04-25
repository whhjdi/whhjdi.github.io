---
title: 深入Vue-RenderFunction
date: 2019-04-10 13:25:36
tags:
cover_img: https://i.loli.net/2019/08/21/lc5eEv8MiRLWnKA.jpg
feature_img:
description:
keywords:
---

# Render Function

## 3.1 Dynamically Render Tags

template =>编译成Render Function => 返回一个新的虚拟DOM => 生成真实的DOM

- 真实DOM
  
```js
document.createElement('div')

`[object HTMLDivElement]`
```

- 虚拟DOM
  
```js
vm.$createElement('div')

`{tag: 'div',data:{attrs:{},...},children:[]}`
```

- h function
  
```js
h('div',{class: 'foo'},'some text')
h('div',{class: 'foo'},['some text',h('span','bar')])
```

### 目标

实现满足以下用法的"example"组件:

``` html
<example :tags="['h1', 'h2', 'h3']"></example>
```

which renders the expected output:

``` html
<div>
  <h1>0</h1>
  <h2>1</h2>
  <h3>2</h3>
</div>
```

You should be using render functions, obviously. The detailed usage can be found in the [docs](https://vuejs.org/v2/guide/render-function.html#createElement-Arguments).

### 实现 

```html
<script src="../node_modules/vue/dist/vue.js"></script>

<div id="app">
	<example :tags="['h1', 'h2', 'h3']"></example>
</div>

<script>
Vue.component('example', {
 functional: true,
 props: {
   tags: {
     type: Array,
     validator (arr) { return !!arr.length }
   }
 },
 render: (h, context) => {
   const tags = context.props.tags
   return h('div', context.data, tags.map((tag, index) => h(tag, index)))
 }
})

new Vue({ el: "#app" });
</script>
```

## 3.2 Dynamically Render Components

### 目标

1. Implement a `Foo` component which simply renders `<div>foo</div>`, and a `Bar` component which simply renders `<div>bar</div>` (using render functions, obviously).

2. Implement an `<example>` component which renders the `Foo` component or the `Bar` component based on its `ok` prop. For <example> if `ok` is true, the final rendered dom should be `<div>foo</div>`.

3. Implement a button in the root component that toggles `<example>` between `Foo` and `Bar` by controlling its `ok` prop.

### 实现 

```html
<script src="../node_modules/vue/dist/vue.js"></script>

<div id="app">
	<example :ok="ok"></example>
	<button @click="ok = !ok"></button>
</div>

<script>
	const Foo = {
		functional: true,
		render: h => h("div", "foo")
	};

	const Bar = {
		functional: true,
		render: h => h("div", "bar")
	};

	Vue.component("example", {
		functional: true,
		props: {
			ok: {
				type: Boolean
			}
		},
		render: (h, context) => h(context.props.ok ? Foo : Bar)
	});

	new Vue({
		el: "#app",
		data: {
			ok: true
		}
	});
</script>
```

## 3.3 Higher Order Component

### 目标

Implement a `withAvatarURL` helper which takes an inner component that expects a `url` prop, and return a higher-order component that accepts a `username` prop instead. The higher-order component should be responsible for fetching the corresponding avatar url from a mocked API.

Before the API returns, the higher-order component should be passing a placeholder URL `http://via.placeholder.com/200x200` to the inner component.

The exercise provides a base `Avatar` component. The usage should look like this:

``` js
const SmartAvatar = withAvatarURL(Avatar)
```

So instead of:

``` html
<avatar url="/path/to/image.png"></avatar>
```

You can now do:

``` html
<smart-avatar username="vuejs"></smart-avatar>
```

### 实现

```html
<script src="../node_modules/vue/dist/vue.js"></script>

<div id="app">
	<smart-avatar username="vuejs"></smart-avatar>
</div>

<script>
	// mock API
	function fetchURL(username, cb) {
		setTimeout(() => {
			// hard coded, bonus: exercise: make it fetch from gravatar!
			cb("https://avatars3.githubusercontent.com/u/6128107?v=4&s=200");
		}, 500);
	}

	const Avatar = {
		props: ["src"],
		template: `<img :src="src">`
	};

	function withAvatarURL(InnerComponent) {
		return {
			props: { username: String },
			data() {
				return {
					url: "http://via.placeholder.com/200x200"
				};
			},
			created() {
				fetchURL(this.username, url => {
					this.url = url;
				});
			},
			render(h) {
				return h(InnerComponent, { props: { src: this.url } });
			}
		};
	}

	const SmartAvatar = withAvatarURL(Avatar);

	new Vue({
		el: "#app",
		components: { SmartAvatar }
	});
</script>

```
---
title: 深入Vue-国际化
date: 2019-04-10 18:13:57
tags:
cover_img: https://i.loli.net/2019/08/21/lc5eEv8MiRLWnKA.jpg
feature_img:
description:
keywords:
---

# 国际化

`i18n`意思就是i开头，n结尾，中间还有18个字母，23333

```html
<script src="../node_modules/vue/dist/vue.js"></script>

<div id="app">
	<h1>{{ $t("welcome-message") }}</h1>
	<button @click="changeLang('en')">English</button>
	<button @click="changeLang('zh')">中文</button>
	<button @click="changeLang('nl')">Dutch</button>
</div>

<script>
	const i18nPlugin = {
		// Implement this!
		install(Vue, locales) {
			Vue.prototype.$t = function(id) {
				console.log(id);

				return locales[this.$root.lang][id];
			};
		}
	};

	Vue.use(
		i18nPlugin,
		/* option */ {
			en: { "welcome-message": "hello" },
			zh: { "welcome-message": "你好" },
			nl: { "welcome-message": "Hallo" }
		}
	);

	new Vue({
		el: "#app",
		data: {
			lang: "zh"
		},
		methods: {
			changeLang(lang) {
				this.lang = lang;
			}
		}
	});
</script>

```

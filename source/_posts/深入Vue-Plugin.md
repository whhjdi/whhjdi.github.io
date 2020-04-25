---
title: 深入Vue-Plugin
date: 2019-04-10 13:05:19
tags:
cover_img: https://i.loli.net/2019/08/21/lc5eEv8MiRLWnKA.jpg
feature_img:
description:
keywords:
---

# Plugin

## 2.1 Simple Plugin

### Goal

Create a plugin that teaches Vue components to handle a custom "rules"
option. The "rules" option expects an object that specifies validation rules
for data in the component.

Expected usage:

``` js
const vm = new Vue({
  data: { foo: 10 },
  rules: {
    foo: {
      validate: value => value > 1,
      message: 'foo must be greater than one'
    }
  }
})

vm.foo = 0 // should log: "foo must be greater than one"
```

### Hints

1. The plugin should install a global mixin
2. The global mixin contains a "created" hook
3. In the hook, check for `this.$options.rules`

### 实现

```js
	const RulesPlugin = {
	  install (Vue) {
	    Vue.mixin({
	      created () {
	        if (this.$options.hasOwnProperty('rules')) {
	          // Do something with rules
	          const rules = this.$options.rules
	          Object.keys(rules).forEach(key => {
	            const rule = rules[key]
	            this.$watch(key, newValue => {
	              const result = rule.validate(newValue)
	              if (!result) {
	                console.log(rule.message)
	              }
	            })
	          })
	        }
	      }
	    })
	  }
	}

	Vue.use(RulesPlugin)
```

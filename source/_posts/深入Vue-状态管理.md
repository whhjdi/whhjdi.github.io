---
title: 深入Vue-状态管理
date: 2019-04-10 15:19:52
tags:
cover_img: https://i.loli.net/2019/08/21/lc5eEv8MiRLWnKA.jpg
feature_img:
description:
keywords:
---

# State Management

如何来管理一个公共的数据呢？
我们可以有很多方法

## 4.1 通过props
```html
<script src="../node_modules/vue/dist/vue.js"></script>

<div id="app">
  <counter :count="count"></counter>
  <counter :count="count"></counter>
  <counter :count="count"></counter>
  <button @click="count++">increment</button>
</div>

<script>
// requirement: a counter component rendered 3 times
// the component takes the current count via props
// and a button that increments all 3 counters at once
new Vue({
  el: '#app',
  data: {
    count: 0
  },
  components: {
    Counter: {
      props: {
        count: Number
      },
      template: `<div>{{ count }}</div>`
    }
  }
})
</script>
```

## 4.2 通过共享一个对象

```html
<script src="../node_modules/vue/dist/vue.js"></script>

<div id="app">
  <counter></counter>
  <counter></counter>
  <counter></counter>
  <button @click="inc">increment</button>
</div>

<script>
// create a counter component (that doesn't take any props)
// all instances of it should share the same count state
// and a button that increments all counters at the same time

const state = {
  count: 0
}

const Counter = {
  // Convert state into reactive object
  data () {
    return state
  },
  render (h) {
    // Proxy the object
    return h('div', this.count)
  }
}

new Vue({
  el: '#app',
  components: {
    Counter
  },
  methods: {
    inc () {
      state.count++
    }
  }
})
</script>
```

## 4.3 通过共享一个store实例

```html
<script src="../node_modules/vue/dist/vue.js"></script>

<div id="app">
	<counter></counter>
	<counter></counter>
	<counter></counter>
	<button @click="inc">increment</button>
</div>

<script>
	// copy and modify the first exercise to use a Vue instance as
	// a shared store instead.
	const state = new Vue({
		data: {
			count: 0
		},
		methods: {
			inc() {
				this.count++;
			}
		}
	});

	const Counter = {
		render: h => h("div", state.count)
	};

	new Vue({
		el: "#app",
		components: {
			Counter
		},
		methods: {
			inc() {
				state.inc();
			}
		}
	});
</script>

```

## 实现一个createStore(类似vuex)

```html
<script src="../node_modules/vue/dist/vue.js"></script>

<div id="app">
  <counter></counter>
  <counter></counter>
  <counter></counter>
  <button @click="inc">increment</button>
</div>

<script>
function createStore ({ state, mutations }) {
  return new Vue({
    data: {
      state
    },
    methods: {
      commit (mutation) {
        if (!mutations.hasOwnProperty(mutation)) {
          throw new Error('Unknown mutation')
        }
        mutations[mutation](state)
      }
    }
  })
}

const store = createStore({
  state: { count: 0 },
  mutations: {
    inc (state) {
      state.count++
    }
  }
})

const Counter = {
  render (h) {
    return h('div', store.state.count)
  }
}

new Vue({
  el: '#app',
  components: { Counter },
  methods: {
    inc () {
      store.commit('inc')
    }
  }
})
</script>

```
## 封装一个函数

```html
<script src="../node_modules/vue/dist/vue.js"></script>

<div id="app"></div>

<script>
function app ({ el, model, view, actions }) {
  const wrappedActions = {}

  Object.keys(actions).forEach(key => {
    const originalAction = actions[key]
    wrappedActions[key] = () => {
      vm.model = originalAction(vm.model)
    }
  })

  const vm = new Vue({
    el,
    data: {
      model
    },
    render (h) {
      return view(h, this.model, wrappedActions)
    },
    methods: actions
  })
}

// voila
app({
  el: '#app',
  model: {
    count: 0
  },
  actions: {
    inc: ({ count }) => ({ count: count + 1 }),
    dec: ({ count }) => ({ count: count - 1 })
  },
  view: (h, model, actions) => h('div', { attrs: { id: 'app' }}, [
    model.count, ' ',
    h('button', { on: { click: actions.inc }}, '+'),
    h('button', { on: { click: actions.dec }}, '-')
  ])
})
</script>

```
这样就封装好了,嘿嘿嘿
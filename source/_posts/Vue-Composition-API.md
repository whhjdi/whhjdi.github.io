---
title: Vue-Composition-API
date: 2019-10-01 17:31:02
tags: [vue]
cover_img:
feature_img:
description:
keywords:
---

# Vue Composition API

> 2020.5.1 vue3 已经进入 beta 阶段了，有些 api 发生了改变，建议直接看之后的文档

[vueconf 尤雨溪的演讲视频及资料](https://www.yuque.com/vueconf/2019/gwn1z0#Mit9a)

## setup

`setup`是一个显得 options，它是在 vue 组件中使用 Composition API 的入口

- 调用时机

  在创建组件实例时，`setup`比 props 初始化要早。在生命周期方面，它在`beforecreate`钩子之后和`created`钩子之前被调用。

```vue
<template>
  <div>{{ count }} {{ object.foo }}</div>
</template>

<script>
import { ref, reactive } from "@vue/composition-api";

export default {
  setup() {
    const count = ref(0);
    const object = reactive({ foo: "bar" });

    // 返回的对象里面的东西会暴露给`template`直接使用
    return {
      count,
      object,
    };
  },
};
</script>
```

```vue
//同样的也支持返回render function
<script>
import { h, ref, reactive } from "vue";

export default {
  setup() {
    const count = ref(0);
    const object = reactive({ foo: "bar" });

    return () => h("div", [count.value, object.foo]);
  },
};
</script>
```

`setup` 函数可以接收 props 作为第一个参数,并且这个`props`是响应式的，当传入新的 props 时，props 会更新

这个更新可以被 watch 监听到。和以前一样，子组件是不能直接修改 props 的，如果用户代码试图改变它，它将发出警告

第二个参数提供了一个上下文对象，它包含了在 2 .x API 中公开的属性列表：

```vue
const MyComponent = { setup(props, context) { context.attrs context.slots
context.parent context.root context.emit } }
```

> 当需要获得对 setup 参数的类型推断的能力，需要使用`createComponent`

## reactive

返回一个响应式的代理对象（proxy）

## ref

获取一个内部值并返回一个响应式的可变的 ref 对象。ref 对象有一个指向内部值的属性.value。

在模板中访问它的值不需要`refName.value`,直接`{{refName}}`

在响应式对象中访问或者修改它时，他的值会自动改变

```vue
const count = ref(0) const state = reactive({ count }) console.log(state.count)
// 0 state.count = 1 console.log(count.value) // 1
```

当然了 ref 也可以去绑定对应的 dom 元素

```vue
<template>
  <div ref="root"></div>
</template>

<script>
import { ref, onMounted } from "vue";

export default {
  setup() {
    const root = ref(null);

    onMounted(() => {
      // dom元素会在initial render之后赋值给ref
      console.log(root.value); // <div/>
    });

    return {
      root,
    };
  },
};
</script>
```

## isRef

检查一个对象是否是 ref 对象,灰常方便

```vue
const unwrapped = isRef(foo) ? foo.value : foo
```

## toRefs

把一个普通的响应式对象转换为 ref 对象，toRefs 会返回一个对象，key 和原来的一样，value 都变成了 ref 对象,

并且此时的 ref 和原来的对象属性绑定了，当修改一个，另一个也会相应的改变

```vue
const state = reactive({ foo: 1, bar: 2 }) const stateAsRefs = toRefs(state) /*
Type of stateAsRefs: { foo: Ref<1>, bar: Ref<2> } */ // The ref and the original
property is "linked" state.foo++ console.log(stateAsRefs.foo) // 2
stateAsRefs.foo.value++ console.log(state.foo) // 3
```

当你声明的复合函数要返回一个响应式对象时，可以使用`toRefs`进行转换，让这个对象在不失去响应式的情况下方便的对对象解构、扩展

```vue
function useFeatureX() { const state = reactive({ foo: 1, bar: 2 }) // logic
operating on state // convert to refs when returning return toRefs(state) }
export default { setup() { // can destructure without losing reactivity const {
foo, bar } = useFeatureX() return { foo, bar } } }
```

## computed

他接收一个回调函数，这个函数返回一个不可变（只读）的响应式数据，同样的他也可以接收有 get,set 函数的对象，此时他就是可写的了

```vue
<template>
  <div>
    <h2>computed</h2>
    <div>plusOne is {{plusOne}}</div>
    <div>plusTwo : {{count2}}+ 2 = {{plusTwo}}</div>
  </div>
</template>

<script>
import {
  ref,
  computed
} from "@vue/composition-api";

export default {
  setup() {
    const count1 = ref(0);
    const count2 = ref(1);
    const plusOne = computed(() => count1.value + 1);
    const plusTwo = computed({
      get: () => {
        return count2.value;
      },
      set: val => {
        return (count2.value = val + 2);
      }
    });
    console.log(plusOne);
    console.log((plusTwo.value = 10));
    return {
      plusOne,
      count2,
      plusTwo
    };
  }
};
```

## readonly

他接收一个响应式对象或者 ref 返回一个只读的对象，这个对象同样是响应式的，当修改源对象时，这个拷贝的对象对应的属性值也会相应的改变，但是不能直接修改，因为是只读的，demo 目前跑不起来

## watch

在下一个 tick 执行，并且当依赖变化时，执行相应的回调函数

第一个参数是要监听的 getter，第二个参数对应的回调，也可以直接监听一个 ref，还可同时监听多个 ref

watch()返回一个停止函数，调用它便可以停止监听

```vue
<template>
  <div>
    <h2>watch</h2>
    <div>{{ state.count }}</div>
    <div>{{ name }}</div>
  </div>
</template>

<script>
import {
  ref,
  reactive,
  watch,
  isRef,
  toRefs,
  computed,
} from "@vue/composition-api";

export default {
  setup() {
    const state = reactive({ count: 0, age: 16 });
    const name = ref("我的名字是🦑");
    const stop = watch(() => state.age);
    console.log("开始监听age");
    setTimeout(() => {
      stop();
      console.log("停止监听age");
    }, 3000);

    watch(
      () => state.count,
      (count, prevVal) => {
        console.log("监听state的count：" + count, prevVal);
      }
    );
    watch(name, (name, prevVal) => {
      // 这个name不需要name.value
      console.log("监听ref,name:" + name, prevVal);
    });
    //     watch([fooRef, barRef], ([foo, bar], [prevFoo, prevBar]) => {
    //   /* ... */
    // })
    return {
      state,
      name,
    };
  },
};
</script>
```

watch 这里还有一些知识，比如异步，清理副作用，等等之类的东西，这些等正式发布再研究吧

## 生命周期

```vue
import { onMounted, onUpdated, onUnmounted } from '@vue/composition-api' const
MyComponent = { setup() { onMounted(() => { console.log('mounted!') })
onUpdated(() => { console.log('updated!') }) onUnmounted(() => {
console.log('unmounted!') }) } }
```

和 vue2.6.x 的生命周期的对比

- beforeCreate -> use setup()
  created -> use setup()
  beforeMount -> onBeforeMount
  mounted -> onMounted
  beforeUpdate -> onBeforeUpdate
  updated -> onUpdated
  beforeDestroy -> onBeforeUnmount
  destroyed -> onUnmounted
  errorCaptured -> onErrorCaptured

新增

- `onRenderTracked`
- onRenderTriggered

## `provide`&`inject`

## v-for

基本用法和以前差不多的，文档现在不全 ，更具体的等正式发布

```vue
<template>
  <div
    v-for="(item, i) in list"
    :ref="
      (el) => {
        divs[i] = el;
      }
    "
  >
    {{ item }}
  </div>
</template>

<script>
import { ref, reactive, onBeforeUpdate } from "vue";

export default {
  setup() {
    const list = reactive([1, 2, 3]);
    const divs = ref([]);

    // make sure to reset the refs before each update
    onBeforeUpdate(() => {
      divs.value = [];
    });

    return {
      list,
      divs,
    };
  },
};
</script>
```

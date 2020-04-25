---
title: 深入Vue-Reactivity
date: 2019-04-09 17:07:27
tags:
cover_img: https://i.loli.net/2019/08/21/lc5eEv8MiRLWnKA.jpg
feature_img:
description:
keywords:
---

# Reactivity

## 1.1 Getters and Setters

### 目标

实现一个 `convert` 函数:

- 接收一个 Object 作为参数

- 使用 object.defineProperty 将对象的属性就地转换为 getter/setter
  [mdn](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty`)

- 转换后的对象应保留原始行为，但同时记录所有的 get/set 操作。

测试用例:

```js
const obj = { foo: 123 };
convert(obj);

obj.foo; // should log: 'getting key "foo": 123'
obj.foo = 234; // should log: 'setting key "foo" to: 234'
obj.foo; // should log: 'getting key "foo": 234'
```

测试实现是否正确, 运行:

```bash
npm test -- -t 1.1
```

### 实现

```js
  
function isObject(obj) {
    return (
      typeof obj === "object" &&
      !Array.isArray(obj) &&
      obj !== null &&
      obj !== undefined
    );
  }

function convert(obj) {
  if (!isObject(obj)) {
    throw new TypeError();
  }

  Object.keys(obj).forEach(key => {
    let internalValue = obj[key];
    Object.defineProperty(obj, key, {
      get() {
        console.log(`getting key "${key}": ${internalValue}`);
        return internalValue;
      },
      set(newValue) {
        console.log(`setting key "${key}" to: ${newValue}`);
        internalValue = newValue;
      }
    });
  });
}
```

## 1.2 Dependency Tracking

### 目标

- Create a `Dep` class with two methods: `depend` and `notify`.
- Create an `autorun` function that takes an updater function.
- Inside the updater function, you can explicitly depend on an instance of `Dep` by calling `dep.depend()`
- Later, you can trigger the updater function to run again by calling `dep.notify()`.

The full usage should look like this:

``` js
const dep = new Dep()

autorun(() => {
  dep.depend()
  console.log('updated')
})
// should log: "updated"

dep.notify()
// should log: "updated"
```

To test if your implementation is correct, run:

``` bash
npm test -- -t 1.2
```

### 实现

```js
window.Dep = class Dep {
	constructor(){
		this.subscribers = new Set()
	}
	depend(){
		if(activeUpdate){
			this.subscribers.add(activeUpdate)
		}
	}
	notify(){
		this.subscribers.forEach(sub=>sub())
	}
}

let activeUpdate

function autorun(update){
	function wrappedUpdate(){
		activeUpdate = wrappedUpdate
		update()
		activeUpdate = null
	}
	wrappedUpdate()
}
```

## 1.3 Mini Observer

### 目标

Combine the previous two functions, renaming `convert()` to `observe()` and keeping `autorun()`:

- `observe()` converts the properties in the received object and make them
  reactive. For each converted property, it gets assigned a `Dep` instance which keeps track of a list of subscribing update functions, and triggers them to re-run when its setter is invoked.

- `autorun()` takes an update function and re-runs it when properties that the
  update function subscribes to have been mutated. An update function is said
  to be "subscribing" to a property if it relies on that property during its
  evaluation.

They should support the following usage:

``` js
const state = {
  count: 0
}

observe(state)

autorun(() => {
  console.log(state.count)
})
// should immediately log "count is: 0"

state.count++
// should log "count is: 1"
```

To test if your implementation is correct, run:

``` bash
npm test -- -t 1.3
```
###  实现

```js
function isObject(obj){
  return (
    typeof obj === 'object' &&
    !Array.isArray(obj) &&
    obj !== null &&
    obj !== undefined
  )
}
function observe(obj){
  if(!isObject(obj)){
    throw new TypeError()
  }
  Object.keys(obj).forEach(key=>{
    let internalValue = obj[key]
    let dep = new Dep()
    Object.defineProperty(obj,key,{
      get(){
        dep.depend()
        return internalValue
      },
      set(newValue){
        internalValue = newValue
        dep.notify()
      }
    })
  })
}
class Dep{
  constructor(){
    this.subscribers = new Set()
  }
  depend(){
    if(activeUpdate){
      this.subscribers.add(activeUpdate)
    }
  }
  notify(){
    this.subscribers.forEach(sub=>sub())
  }
}

let activeUpdate
function autorun(update){
  function wrappedUpdate(){
    activeUpdate = wrappedUpdate
    update()
    activeUpdate = null
  }
  wrappedUpdate()
}
```

 以上就实现了一个简单的观察者
---
title: MVC与单向绑定和双向绑定
date: 2018-08-12 20:38:04
tags: [MVC]
categories: ["其他"]
cover_img: https://ws4.sinaimg.cn/large/006tNbRwly1fxzeangnl0j31hc0u0u11.jpg
---

# MVC

## MVC 的出现

举栗子
如果没有 MVC 代码是怎样的呢?
没错就是意大利面条

<!--more-->

```javascript
axios.interceptors.response.use(function(response) {
  let {
    config: { url, method, data }
  } = response;
  data = JSON.parse(data || "{}");
  let row = {
    id: 1,
    name: "JavaScript 高级程序设计",
    number: 2
  };
  if (url === "/books/1" && method === "get") {
    response.data = row;
  } else if (url === "/books/1" && method === "put") {
    response.data = Object.assign(row, data);
  }
  return response;
});
// <!-- 专门负责操作远程数据的代码 -->

function fetchDb() {
  return axios.get("/books/1");
}

function saveDb(newData) {
  return axios.put("/books/1", newData);
}

// <!-- 专门呈现页面元素的代码 -->
var template = `

<div>
  书名：《__name__》，
  数量：<span id="number">__number__</span>
</div>
<div class="actions">
  <button id="increaseByOne">加1</button>
  <button id="reset">归零</button>
</div>
`;
fetchDb().then(response => {
  let result = response.data;
  $("#app").html(
    template
      .replace("__number__", result.number)
      .replace("__name__", result.name)
  );
  // <!-- 其他逻辑代码 -->
  //加1
  $("#increaseByOne").on("click", e => {
    let oldResult = parseInt($("#number").text(), 10);
    let newResult = oldResult + 1;
    saveDb({ number: newResult }).then(function({ data }) {
      console.log(data);
      $("#number").text(data.number);
    });
  });

  //归零
  $("#reset").on("click", e => {
    let newResult = 0;
    saveDb({ number: newResult }).then(({ data }) => {
      $("#number").text(data.number);
    });
  });
});
```

聪明的前端程序员发现这些代码总是可以分为三类(类似于后端的分类)

专门操作远程数据的代码（fetchDb 和 saveDb 等等）
专门呈现页面元素的代码（innerHTML 等等）
其他控制逻辑的代码（点击某按钮之后做啥的代码）
慢慢的被大家完善完善,最终形成了现在的 MVC 思想

M 专门负责数据
V 专门负责表现
C 负责其他逻辑
ok,看懂上面的代码,我们就可以尝试把这些代码改写成 MVC 的模式

## 改写成 MVC

分成三个对象 view model controller
model 只负责存储数据,请求数据,更新数据
view 只负责渲染 HTML
controller 负责调度 mdoel 和 view
看代码

```javascript
let model = {
  data: {
    number: 0,
    name: ""
  },
  fetch(id) {
    return axios.get(`/books/${id}`).then(response => {
      this.data = response.data;
    });
  },
  update(newData) {
    let id = this.data.id;
    return axios.put(`/books/${id}`, newData).then(({ data }) => {
      this.data = data;
    });
  }
};

let view = {
  el: "#app",
  template: `
<div>
  书名：《__name__》，
  数量：__number__
</div>
<div class="actions">
  <button id="increaseByOne">加1</button>
  <button id="reset">归零</button>
</div>
  `,
  render(data) {
    let html = this.template
      .replace("__name__", data.name)
      .replace("__number__", data.number);
    console.log(data);
    $(this.el).html(html);
  }
};

var controller = {
  init({ view, model }) {
    this.view = view;
    this.model = model;
    this.view.render(this.model.data);
    this.bindEvents();
    console.log(1);
    this.fetchModel();
    console.log(2);
  },
  events: [
    { type: "click", selector: "#increaseByOne", fnName: "add" },
    { type: "click", selector: "#reset", fnName: "reset" }
  ],
  bindEvents() {
    this.events.map(event => {
      $(this.view.el).on(
        event.type,
        event.selector,
        this[event.fnName].bind(this)
      );
    });
  },
  add() {
    let newData = { number: this.model.data.number + 1 };
    this.updateModel(newData);
  },

  fetchModel() {
    this.model.fetch(1).then(() => {
      this.view.render(this.model.data);
    });
  },
  updateModel(newData) {
    this.model.update(newData).then(() => {
      this.view.render(this.model.data);
    });
  }
};
controller.init({ view, model });
```

嘿嘿嘿初步的 MVC 就实现了,这真是个好套路,没错都是套路
BUT
如果你有好多页面,那么每个页面就都有各自的 MVC,有重复就能优化
所以,我们用面向对象的方式来改进一下,把共有的属性放到原型里
(也就是把重复的代码放到一个类里)

## 面向对象

```javascript
class Model  {
  constructor({resource,data}){
    this.resource = resource
    this.data = data
  },
  fetch(id) {
    return axios.get(`/${resource}s/${id}`).then((response)=>{
      this.data = response.data
    })
  },
  update(newData) {
    let id = this.data.id
    return axios.put(`/${resource}s/${id}`, newData).then(({data})=>{
      this.data = data
    })
  }
}
// <!-- 这样就可以new很多不同的model,他们的共有属性都在Model类里 -->
let model = new Model({
  resource:'book',
  data:{
    number:0,
    name:''
  }
})

class View {
  constructor({template,el}){
    this.template = template
    this.el = el
    this.$el = $(this.el)
  }
  render(data) {
    let html = this.template
    for(let key in data){
      html = html.replace(__`${key}`__,data[key])
      this.$el.html(html)
    }
  }
}
// <!-- 现在可以new view了 -->
let view = new View({
  template: `
<div>
  书名：《__name__》，
  数量：__number__
</div>
<div class="actions">
  <button id="increaseByOne">加1</button>
  <button id="reset">归零</button>
</div>
  `,
  el: '#app',
})

class Controller{
  constructor({view,model,events,...rest}){
    this.view = view
    this.model = model
    this.events = events
    Object.assign(this,rest)
    this.bindEvents()
    this.view.render(this.model.data)
    init.apply(this,arguments)
  }
  bindEvents() {
    this.events.map((e) => {
      this.view.$el.on(e.type, e.el, this[e.fn].bind(this))
    })
  }
}
var controller = new Controller({
  view: view,
  model: model,
  events: [
    { type: 'click', el: '#increaseByOne', fn: 'add' },
    { type: 'click', el: '#reset', fn: 'reset' }
  ],
  // 这里可以优化1
  init(options) {
    this.model.fetch(1)
      .then(() => {
        this.view.render(this.model.data)
      })
  },
  add() {
    let newData = {number: this.model.data.number + 1}
    this.updateModel(newData)
  },
  reset() {
    this.updateModel({number: 0})
  },
  updateModel(newData) {
    // 1
    this.model.update(newData).then(()=>{
      this.view.render(this.model.data)
    })
  }
// <!-- controller相对复杂,好好看看 -->
```

每次更新数据都要手动 render,这样不好…
怎么优化呢?引入发布订阅模式
通过事件机制,每次数据更新,就自动触发一个事件,
这个事件会自动渲染 data,看代码

## 发布订阅

```javascript
class EventHub {
  constructor() {
    this.events = {};
  }
  on(eventName, fn) {
    if (!this.events[eventName]) {
      this.events[eventName] = [];
    }
    this.events[eventName].push(fn);
  }
  emit(eventName, params) {
    let fnList = this.events[eventName];
    fnList.forEach(fn => {
      fn.apply(null, params);
    });
  }
  off(eventName, fn) {
    let fnList = this.events[eventName];
    for (let i = 0; i < fnList.length; i++) {
      if (fnList[i] === fn) {
        delete fnList[i];
        break;
      }
    }
  }
}
```

好的下面介绍一下这个事件中心

有一个 events 负责存储事件名和事件对应的函数
方法 on 就负责判断你穿进来的事件在不在 events 里
如果不在,就把这个事件对应一个空数组
然后把传进来的函数 push 到这个事件名对应的数组里
就像这样 events = {eventName:[fn1,fn2]}
那么 emit 是什么呢?就是触发一个事件,也就是调用这个事件对应的函数
首先第一个参数是事件名,它对应一个数组,这个数组里可能有多个函数
所以,我们要遍历这个数组,然后分别调用每一个数组里函数,this 用 apply 绑定成
null 就行
还可能有一个 off,和 emit 差不多,遍历数组 delete 每个函数
有了事件中心,我们就可以让 model 继承这个 EventHub(别忘了写 super())
这样 model 就具有了 on,emit 等这些属性,
然后再 controller 里绑定事件 changed,

```javascript
this.model.on("changed", () => {
  this.view.render(this.model.data);
});
```

更新数据之后,this.emit(‘changed’)

详细代码请戳[这里](https://jsbin.com/sodojac/10/edit?js,output)

基本没问题了,但是 render 函数还不太完美,
如果有个输入框,你每次 render 之后,输入框了的数据就没有了,是的没有了
那么怎么解决呢

把输入的数据记录在 js 的 data 里(数据绑定的思想),这个数据并不是存在数据库里的,
它属于 UI 层面的数据.
哪里变动改哪里,就像哪里不会点哪里一样.不要粗暴的操作 innerHTML(虚拟 DOM 的思想)
为了解决类似这样的 MVC 的 bug,各种框架应运而生
Angular 就是基于第一种数据绑定的思想
React 就是基于第二种虚拟 DOM 的思想

前端花了很长的时间普及内容和行为分离的观点.但是这些框架实在是太方便了…..
了解的 MVVM 我们就可以尝试把代码改写成 vue 的代码

## vue

渐进式(从 MVC 过渡到 vue 比较自然)
双向绑定了解一下
单向数据流了解一下
看文档,文档,档

## React

断崖式(学习曲线比较陡峭)
jsx 了解一下
单向绑定了解一下
redux 了解一下
虚拟 DOM 了解一下
state->render->input->触发 onchange->setState
应该大体就是这个意思

## 双向绑定 Vs 单向绑定

双向绑定实现方式:

赃值检查
数据劫持和发布订阅
Object.defineProperty()无法监听一开始不存在的数据
Proxy 代理(vue 之后会用 Proxy 改写)

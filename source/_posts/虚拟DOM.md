---
title: 虚拟DOM
date: 2018-08-18 17:58:33
tags: [虚拟DOM]
categories: ["其他"]
cover_img: https://ws3.sinaimg.cn/large/006tNbRwly1fxzf1deim7j31hc0u0aa7.jpg
---

# 虚拟 DOM

虚拟 DOM 实际上就是一个 对象，使用它来减少对真实 DOM 的操作次数。对比之后选择更新那些 dom，而不是全部删除再重建

<!--more-->

## 实现一个简单的虚拟 DOM

```javascript
class vNode {
  constructor() {
    this.tag = tag;
    this.text = text;
    this.children = children;
  }
  render() {
    if (this.tag === "#text") {
      return document.createTextNode(this.text);
    }
    let el = document.createElement(this.tag);
    this.children.forEach(vChild => {
      el.appendChild(vChild.render());
    });
    return el;
  }
}
function v(tag, children, text) {
  if (typeof children === "string") {
    text = children;
    children = [];
  }
  return new VNode(tag, children, text);
}
function patchElement(parent, newVNode, oldVNode, index = 0) {
  if (!oldVNode) {
    parent.appendChild(newVNode.render());
  } else if (!newVNode) {
    parent.removeChild(parent.childNodes[index]);
  } else if (newVNode.tag !== oldVNode.tag || newVNode.text !== oldVNode.text) {
    parent.replaceChild(newVNode.render(), parent.childNodes[index]);
  } else {
    for (
      let i = 0;
      i < newVNode.children.length || i < oldVNode.children.length;
      i++
    ) {
      patchElement(
        parent.childNodes[index],
        newVNode.children[i],
        oldVNode.children[i],
        i
      );
    }
  }
}

let vNodes1 = v("div", [
  v("p", [v("span", [v("#text", "hello world")])]),
  v("span", [v("#text", "muxue")])
]);

let vNodes2 = v("div", [
  v("p", [v("span", [v("#text", "hello world")])]),
  v("span", [v("#text", "1231"), v("#text", "喵喵")])
]);
const root = document.querySelector("#root");
patchElement(root, vNodes1);
```

实际的 diff 算法要复杂的多，我只是简单理解一下虚拟 DOM

## react 中的虚拟 DOM

1. state 数据
2. JSX 模板
3. 生成虚拟 DOM `['div,{id='abc'},['span',{},'hello']]`
4. 用虚拟 DOM 的结构生成真实 DOM 来显示
5. state 发生变化
6. 生成新的虚拟 DOM `['div,{id='abc'},['span',{},'bye']]`
7. 比较原始虚拟 DOM 和新的虚拟 DOM 的区别
8. 直接操作 DOM,改变 span 中的内容

### 优点

1. 提升性能
2. 使跨端应用得以实现

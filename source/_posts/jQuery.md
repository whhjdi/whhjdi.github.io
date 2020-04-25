---
title: jQuery
date: 2018-07-16 18:19:51
tags: [jQuery]
categories: ["其他"]
cover_img: https://ws2.sinaimg.cn/large/006tNbRwly1fxzf2iu5m6j30us0ig77h.jpg
---

## 实现一个 jQuery 的 API

实现一个函数,并判断参数是节点还是选择器,返回一个 nodes(数组形式的对象)
让 addClass()可以同时增加几个 className,把 setText 变成 text(既能 getText,又能 setText)
addClass()传进一个数组,获取数组的每个 value,然后遍历 nodes,为 nodes[i]添加 class —value
text(),判断参数是不是 undefined,如果是 undefined 的就是要 getText,否则就是 setText

<!--more-->

```javascript
window.jQuery = function(nodeOrSelector) {
  let nodes = {};
  if (typeof nodeOrSelector === "string") {
    let temp = document.querySelectorAll(nodeOrSelector);
    for (let i = 0; i < temp.length; i++) {
      nodes[i] = temp[i];
    }
    nodes.length = temp.length;
  } else if (nodeOrSelector instanceof Node) {
    nodes = { 0: nodeOrSelector, length: 1 };
  }

  nodes.addClass = function(classes) {
    classes.forEach(value => {
      for (let i = 0; i < nodes.length; i++) {
        nodes[i].classList.add(value);
      }
    });
  };
  nodes.text = function(text) {
    if (text === undefined) {
      let texts = [];
      for (let i = 0; i < nodes.length; i++) {
        texts.push(nodes[i].textContent);
      }
    } else {
      for (let i = 0; i < nodes.length; i++) {
        nodes[i].textContent = text;
      }
    }
  };
  return nodes;
};

window.$ = jQuery;
var $div = $("div");
$div.addClass("red");
$div.text("hi");
```

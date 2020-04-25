---
title: 我知道的JS-DOM
date: 2018-04-20 17:30:00
tags: [JavaScript]
categories: ["Javascript"]
cover_img: https://ws1.sinaimg.cn/large/006tNbRwly1fy1lxezoixj30wg0mzkjl.jpg
---

# DOM

> DOM 是 JavaScript 操作网页的接口，全称为“文档对象模型”（Document Object Model）。它的作用是将网页转为一个 JavaScript 对象，从而可以用脚本进行各种操作（比如增删内容）。

## 节点

DOM 的最小自称单位叫做节点(node)
节点有七种类型:
Document：整个文档树的顶层节点
DocumentType：doctype 标签（比如<!DOCTYPE html>）
Element：网页的各种 HTML 标签（比如 body 标签,a 标签等）
Attribute：网页元素的属性（比如 class=”right”）
Text：标签之间或标签包含的文本
Comment：注释
DocumentFragment：文档的片段
浏览器提供一个原生的节点对象 Node，上面这七种节点都继承了 Node，因此具有一些共同的属性和方法

<!--more-->



## 操作DOM

```js
document.getElementById()
dom.getElementByTagName()
dom.getElementByClassName()
dom.querySelector()
dom.querySelectorAll()

document.documentELement
doucument.childNodes
children
firstChild lastChild
previousSibing nextSibling
insertBefore


createElement
createTextNode
innerHtml innerText
creatFragmentELement
element.cloneNode()
parent.removeChild()
element.setAttribute('xx',as)
element.getAttribute('xx')
element.removeAttribute('xx')


element.style.xxx = xxx
element.className = xxx


//获取可视区域的宽高（content+padding）
client=> top/left width/height
//（content+padding+border）
offset=> top/left width/height parent
//获取页面真实的高度
scroll=> top/left width/height

document.documentELement.scrollTop=0

window.getComputedStyle(el)
element.currentStyle



```



## 事件模型

监听函数

```html
<body onload="doSomething()">
  <div onclick="console.log('触发事件')">
    <!-- 不推荐 -->
  </div>
</body>
```

```javascript
window.onload = doSomething;
div.onclick = function(event) {
  console.log("触发事件");
};
//如果定义两次onclick属性，后一次定义会覆盖前一次。因此，也不推荐使用。
```

```javascript
window.addEventListener("load", doSomething, false);
```

Event.currentTarget 属性返回事件当前所在的节点，即正在执行的监听函数所绑定的那个节点
Event.target 属性返回原始触发事件的那个节点，即事件最初发生的节点。

事件的传播(DOM2 级事件)

第一阶段：从 window 对象传导到目标节点（上层传到底层），称为“捕获阶段”（capture phase）。
第二阶段：在目标节点上触发，称为“目标阶段”（target phase）。
第三阶段：从目标节点传导回 window 对象（从底层传回上层），称为“冒泡阶段”（bubbling phase）。

阻止冒泡(事件传播)

```javascript
// 事件传播到 p 元素后，就不再向下传播了
p.addEventListener(
  "click",
  function(event) {
    event.stopPropagation();
  },
  true
);

// 事件冒泡到 p 元素后，就不再向上冒泡了
p.addEventListener(
  "click",
  function(event) {
    event.stopPropagation();
  },
  false
);
```

阻止默认事件

```javascript
event.preventDefault;
```

## 常见事件

```js
鼠标事件
onmousedown, onmouseup, onclick, ondbclick, onmousewheel, onmousemove, onmouseover, onmouseout

触摸事件
ontouchstart, ontouchend, ontouchmove

键盘事件：
onkeydown, onkeyup, onkeypress

页面相关事件：
onload, onmove(浏览器窗口被移动时触发), onresize(浏览器的窗口大小被改变时触发), onscroll(滚动条位置发生变化时触发)

表单相关事件
onblur(元素失去焦点时触发), onchange(元素失去焦点且元素内容发生改变时触发), onfocus(元素获得焦点时触发), onreset(表单中 reset 属性被激活时触发), onsubmit(表单被提交时触发)；oninput(在 input 元素内容修改后立即被触发，兼容 IE9+)

编辑事件
onbeforecopy：当页面当前的被选择内容将要复制到浏览者系统的剪贴板前触发此事件；

onbeforecut：当页面中的一部分或者全部的内容将被移离当前页面[剪贴]并移动到浏览者的系统剪贴板时触发此事件；

onbeforeeditfocus：当前元素将要进入编辑状态；

onbeforepaste：内容将要从浏览者的系统剪贴板传送[粘贴]到页面中时触发此事件；

onbeforeupdate：当浏览者粘贴系统剪贴板中的内容时通知目标对象；

oncontextmenu：当浏览者按下鼠标右键出现菜单时或者通过键盘的按键触发页面菜单时触发的事件；

oncopy：当页面当前的被选择内容被复制后触发此事件；

oncut：当页面当前的被选择内容被剪切时触发此事件；

onlosecapture：当元素失去鼠标移动所形成的选择焦点时触发此事件；

onpaste：当内容被粘贴时触发此事件；

onselect：当文本内容被选择时的事件；

onselectstart：当文本内容选择将开始发生时触发的事件；

拖动事件
ondrag：当某个对象被拖动时触发此事件 [活动事件]；

ondragdrop：一个外部对象被鼠标拖进当前窗口时触发；

ondragend：当鼠标拖动结束时触发此事件；

ondragenter：当对象被鼠标拖动的对象进入其容器范围内时触发此事件；

ondragleave：当对象被鼠标拖动的对象离开其容器范围内时触发此事件；

ondragover：当某被拖动的对象在另一对象容器范围内拖动时触发此事件；

ondragstart：当某对象将被拖动时触发此事件；

ondrop：在一个拖动过程中，释放鼠标键时触发此事件；

移动端的触摸事件
touchstart touchmove touchend touchcancel
```

## 事件委托

优点：
可以监听动态生成的元素，省监听器，节省内存
event.currentTaget 始终是监听事件者
event.target 而 target 是事件的真正触发者

```html
<ul id="ul">
  <li><span>1</span></li>
  <li><span>3</span></li>
  <li><span>2</span></li>
  <li><span>4</span></li>
  <li><span>5</span></li>
</ul>
```

```javascript
function listener(element, selector, eventType, fn) {
  //element 是父元素 ul
  element.addEventListener(eventType, e => {
    //el当前点击的元素
    let el = e.target;
    while (!el.matches(selector)) {
      if (element === el) {
        el = null;
        break;
      }
      el = el.parentNode;
    }
    el && fn.call(el, e, el);
  });
  return element;
}
listener(ul, "li", click, () => {});
```

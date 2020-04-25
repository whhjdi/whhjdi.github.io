---
title: 学习JavaScript数据结构与算法
date: 2019-06-15 21:51:47
tags: [JavaScript, 数据结构, 算法]
cover_img:
feature_img:
description:
keywords: [JavaScript, 数据结构, 算法]
---

## 前言

今年打算看两本书，一本是《学习 JavaScript 数据结构与算法》，另一本是《JavaScript 设计模式与开发实践》，先占着坑，慢慢填
2019.6.28 一口气看了 100 多页
2019.6.29 这代码错误有点多啊，变量名前后不一样，index 初始值写错等等

## 基础部分

这本书的基础部分我觉得写得特别好,复习 JS 基础知识可以看看

## 数据结构

### 数组

    数组可能是用的最多的了，用法啥的就没必要写了

### 栈

- 使用数组来模拟一个栈

  ```js
  function Stack(){
      let items = []

      //push
      this.push = function(element){
          items.push(element)
      }
      //pop
      this.pop = function(){
          return items.pop()
      }
      //查看栈顶元素
      this.peek = function(){
          return items[items.length-1]
      }
      //栈的长度
      this.size = function(){
          return items.length
      }
      //检查栈是否为空
      this.isEmpty = function(){
          return items.length === 0
      }
      //清空栈
      this.clear = function(){
          items = []
      }
      //打印
      this.print = function(){
          console.log(items.toString())
      }
  }

  //ES6的方式
  //这种方式是不合适的因为items不是私有属性,可以被修改
  //我们希望的只暴露出想暴露的属性和方法
  class Stack {
      constructor(){
          this.items = []
      }
      push(item){
          this.items.push(item)
      }
  }
  //一个比较合适的方案
  //使用WeakMap和闭包来实现

  let Stack = (function(){
      const items = new WeakMap()
      class Stack {
          constructor(){
              items.set(this,[])
          }
      }
      return Stack
  })()
  //使用

  let stack = new Stack()
  stack.push(1)
  stack.clear()
  ......
  ```

- 使用栈来解决问题

  ```js
  //实现一个进制转换
  function baseConverter(decNumber, base) {
    var remStack = new Stack(),
      rem,
      baseString = "",
      digits = "0123456789ABCDEF"; //对于不同的进制余数需要有一个对应关系

    while (decNumber > 0) {
      rem = Math.floor(decNumber % base);
      remStack.push(rem);
      decNumber = Math.floor(decNumber / base);
    }

    while (!remStack.isEmpty()) {
      baseString += digits[remStack.pop()];
    }

    return baseString;
  }
  ```

### 队列

> 队列是遵循 FIFO（先进先出）原则的一组有序的项。队列在尾部添加新元素，并从顶部移除元素。最新添加的元素必须在队列的末尾

- 实现一个队列

```js
function Queue() {
  let items = [];

  //向尾部添加元素
  this.enqueue = function(item) {
    items.push(item);
  };
  //移除排在最前面的元素
  this.dequeue = function() {
    return items.shift();
  };
  //返回队列中的第一个元素
  this.front = function() {
    return items[0];
  };
  //判断队列是否为空
  this.isEmpty = function() {
    return items.length === 0;
  };
  //返回元素的个数
  this.size = function() {
    return items.length;
  };
  //打印队列的元素
  this.print = function() {
    console.log(items.toString());
  };
}

//使用ES6实现
let Queue = (function() {
  let items = new WeakMap();
  class Queue {
    constructor() {
      //item的key是实例对象，value是一个数组
      items.set(this, []);
    }
    //其他方法
    enqueue(item) {
      let q = items.get(this);
      q.push(item);
    }
    dequeue() {
      let q = items.get(this);
      return q.shift();
    }
    //.......
  }
  return Queue;
})();
//使用

let queue = new Queue();
```

#### 优先队列

> 元素的添加和移除是基于优先级的

```js
/*
实现一个优先队列
设置优先级，然后在正确的位置添加元素
或者有用入列操作添加元素，然后按照优先级移除它
*/

function PriorityQueue() {
  let items = [];
  function QueueElement(element, priority) {
    this.element = element;
    this.priority = ptiority;
  }
  this.enqueue = function(element, priority) {
    let queueEle = new QueueElement(element, priority);
    let added = false;
    for (let i = 0; i < items.length; i++) {
      if (queueEle.priority < items[i].priority) {
        items.splice(i, 0, queueEle);
        added = true;
        break;
      }
      if (!added) {
        items.push(ququeEle);
      }
    }
  };
  //......
}
```

#### 循环队列

```js
//模式实现一个击鼓传花
function hotPotato(nameList, num) {
  let queue = new Queue();
  //把孩子放入队列中
  for (let i = 0; i < nameList.length; i++) {
    queue.enQueue(nameList[i]);
  }

  let eliminated = "";
  //开始传递🌹
  while (queue.size > 1) {
    for (let i = 0; i < num; i++) {
      queue.enqueue(queue.dequeue());
    }
    //淘汰的
    eliminated = queue.dequeue();
  }

  // 胜利者
  return queue.dequeue();
}
```

### 链表

#### 实现一个链表

```js
function LinkedList() {
  let Node = function(item) {
    this.item = item;
    this.next = null;
  };

  let length = 0;
  //存储第一个节点的引用
  this.head = null;

  //向尾部追加
  this.append = function(item) {
    let node = new Node(item);

    let current;
    //判断是否是第一项，如果是第一项，就赋值给head，否则就把head作为当前项current
    if (head === null) {
      head = node;
    } else {
      current = head;
      //如果当前项的next不是null，就到他的下一项
      while (current.next) {
        current = current.next;
      }
      //让最后一项的next指向这个要添加的项
      current.next = node;
    }
    length++;
  };
  this.insert = function(position, item) {
    //让node指向当前项，然后前一项指向node
    if (position > -1 && position < length) {
      let node = new Node(item);
      let current = head;
      let previous;
      let index = 0;

      if (position === 0) {
        node.next = head;
        head = node;
      } else {
        while (index++ < position) {
          previous = current;
          current = current.next;
        }

        node.next = current;
        previous.next = node;
      }
      length++;
      return true;
    } else {
      return false;
    }
  };
  this.removeAt = function(position) {
    //就是让删除位置的前一项的next指向当前项的next
    if (position > -1 && position < length) {
      let current = head;
      let previous;
      let index = 0;

      if (position === 0) {
        head = current.next;
      } else {
        while (index++ < position) {
          previous = current;
          current = current.next;
        }
        previous.next = current.next;
      }

      length--;
      return current.item;
    } else {
      return null;
    }
  };
  this.remove = function(item) {
		let index = this.indexOf(item)
		return this.removeAt(index)
	};
  this.indexOf = function(item) {
		let current = head
		let index = 0
		while(current){
			if(item === current.item){
				return index;
			}
			index++
			current = current.next
		}
		return -1
	};
  this.isEmpty = function() {
		return length===0;
	};
  this.size = function() {
		return length;
	};
  this.getHead = function() {
		return head
	};
  this.toString = function() {
		let current = head
		string = ''
		while(current){
			string +=current.item+ （current.next?'n':''）
			current=current.next
		}
		return string
	};
  this.print = function() {};
}
```

#### 双向链表

双向链表就是一个节点的的链接是双向的，一个指向前一个元素，一个指向后一个元素

```js
function DoublyLinkedList() {
  let Node = function(item) {
    this.item = item;
    this.next = null;
    this.prev = null; //新增的
  };

  let length = 0;
  let head = null;
  let tail = null; //新增的

  //其他方法
  this.insert = function(position, itme) {
    if (position > 0 && position <= length) {
      let node = new Node();
      let current = head;
      let previous;
      let index = 0;

      if (position === 0) {
        if (!head) {
          head = node;
          tail = node;
        } else {
          node.next = current;
          current.previous = node;
          head = node;
        }
      } else if (position === length) {
        current = tail;
        current.next = node;
        node.previous = current;
        tail = node;
      } else {
        while (index++ < position) {
          previous = current;
          current = current.next;
        }
        node.next = current;
        current.previous = node;

        node.previous = previous;
        previous.next = node;
      }
      length++;
      return true;
    } else {
      return false;
    }
  };
  this.removeAt = function(position) {
    //检查越界值
    if (position > -1 && position < length) {
      let current = head,
        previous,
        index = 0;

      //移除第一项
      if (position === 0) {
        head = current.next; // {1}

        //如果只有一项，更新tail //新增的
        if (length === 1) {
          // {2}
          tail = null;
        } else {
          head.prev = null; // {3}
        }
      } else if (position === length - 1) {
        //最后一项 //新增的

        current = tail; // {4}
        tail = current.prev;
        tail.next = null;
      } else {
        while (index++ < position) {
          // {5}

          previous = current;
          current = current.next;
        }

        //将previous与current的下一项链接起来——跳过current
        previous.next = current.next; // {6}
        current.next.prev = previous; //新增的
      }

      length--;

      return current.element;
    } else {
      return null;
    }
  };
}
```

#### 循环链表

循环链表的 tail.next 指向 head
双向循环链表的 tail.next 指向 head 并且 head.previous 指向 tail
循环链表可以是单向的也可以是双向的

### 集合

> 集合是由一组无序且唯一的项组成的
> 集合是一个数学概念，有空集，交集，并集，差集，子集等基本操作和概念

可以使用对象来实现一个集合，不过 ES6 已经有了 Set 和 WeakSet 了
所以就用简单的 Set 来模拟一下集合的一些操作

```js
let setA = new Set();
setA.add(1);
setA.add(2);
setA.add(3);

let setB = new Set();
setB.add(2);
setB.add(3);
setB.add(4);

//并集
let union = new Set();
for (let x of setA) {
  union.add(x);
}
for (let x of setB) {
  union.add(x);
}

//交集
let intersection = function(setA, setB) {
  let intersectionSet = new Set();
  for (let x of setA) {
    if (setB.has(x)) {
      intersectionSet.add(x);
    }
  }
  return intersectionSet;
};

//差集(setA有而setB没有)
let difference = function(setA, setB) {
  let differenceSet = new Set();
  for (let x of setA) {
    if (!setB.has(x)) {
      differenceSet.add(x);
    }
  }
  return differenceSet;
};
```

### 字典和散列表

> 字典是以键值对的形式来存储元素

ES6 也实现了 Map 和 WeakMap,具体的 API 和用法参见 MDN

```js
//一个比较好的散列函数
var djb2HashCode = function(key) {
  var hash = 5381; //{1}
  for (var i = 0; i < key.length; i++) {
    //{2}
    hash = hash * 33 + key.charCodeAt(i); //{3}
  }
  return hash % 1013; //{4}
};
```

### 树

> 它对于存储需要快速查找的数据非常有用

#### 二叉树和二叉搜索树

> 二叉树的节点中最多有两个子节点

```js
//实现一个儿茶搜索树
function BinSearchTree() {
  let Node = function(key) {
    this.key = key;
    this.left = null;
    this.right = null;
  };
  let root = null;
  let insertNode = function(node, newNode) {
    if (newNode.key < node.key) {
      if (node.left === null) {
        node.left = newNode;
      } else {
        insertNode(node.left, newNode);
      }
    } else {
      if (node.right === null) {
        node.right = newNode;
      } else {
        insertNode(node.right, newNode);
      }
    }
  };
  //向树中插入一个键
  this.insert = function(key) {
    let newNode = new Node(key);
    if (root === null) {
      root = newNode;
    } else {
      insertNode(root, newNode);
    }
  };
}
```

#### 树的遍历

- 中序遍历

  

- 前序遍历

- 后序遍历

### 图

	>图是网络结构的抽象模型。是由一组边连接的节点，任何二元耿若曦都可以用图来表示

1. 由一条边连接在一起的顶点称为相邻顶点
2. 一个顶点的度是其相邻顶点的数量
3. 路径是由顶点v1,v2…vk的一个连续的序列
4. 简单路径要求不包含重复的点
5. 除去最后一个顶点，环也是一个简单路径
6. 如果图中不存在环，则称该图是五环的。如果每两个顶点间都存在路径，则称该图是连通的

####  有向图和无向图

如果图中每两个顶点间在双向上都存在路径，则该图是强连通的。

waiting

## 排序和搜索算法

### 二分查找

```js
/*
A:数组
x: 要查找的值
返回： x在A中的位置，找不到返回-1
*/
function bSearch(A, x){
  //左边界
  let l = 0;
  //右边界
  let r = A.length - 1;
  //l,r的中间位置
  let guess
  while(l <= r ){
    guess = Math.floor((l+r)/2);
    //循环不变式
    if(A[guess] === x) return guess;
    else if(A[guess] > x) r = guess-1;
    else l = guess + 1;
    //循环不变式
    //l：新的查找范围左， r:新的查找范围右
  }
  return -1
}

let arr = [1,3,5,7,8,9,123,1234]
bSearch(arr,5)
```

### 插入排序

```js
/*
A: 已排序的数组
x: 需要插入的元素
返回值： 无
*/

function insert(A,i,x){
  // p指向下一个需要比较的元素
  // p+1指向空位
  let p = i -1
  while(p>=0 && A[p]>x){
    A[p+1] = A[p]
    p--
  }
  A[p + 1] = x
}

function insertion_sort(A,x){
  for(let i=0;i>A.length;i++){
    insert(A,i,A[i])
  }

  
}
```

### 冒泡排序

```js
/*
A: 需要排序的数组
返回值： 无
*/
function swap(A,i,j){
  const t= A[i]
  A[i] = A[j]
  A[j] = t
}
function bubble_sort(A){
  for(let i = A.length - 1; i>=1; i--){
    for(let j = 1; i<=i;j++){
      A[j-1]>a[j] && swap(A,j-1,j)
    }
  }
}
```

### 归并排序

```js
/*
T - O(nlgn)
S - O(n)
*/
/*
A:数组
p: 左半边开始
q: 右半边开始，左半边结束
r: 右半边结束
*/
function merge(A, p , q, r){
  let A1 = A.slice(p,q)
  let A2 = A.slice(q,r)
  A1.push(Number.MAX_SAFE_INTEGER)
  A2.push(Number.MAX_SAFE_INTEGER)
  for(let k = p,i=0,j=0;k<r;k++>){
    A[k] = A1[i]<A[j]?A1[i++]:A2[j++]
  }
}

function merge_sort(A,p,r){
  if(r-p < 2 ) return;
  const q = Math.ceil((p+r)/2)
  merge_sort(A,p,q)
  merge_sort(A,q,r)
  merge(A,p,q,r )
}
```

### 快速排序

```js
/*
A:数组
返回值：无
*/
function swap(A,i,j){
  let t = A[i]
  A[i] = A[j]
  A[j] = t
}
function partition(A,lo,hi){
  //小于中心点的范围[lo,i)
  //未确认的范围 [i,j)
  //大于中心点的范围[j,hi-1)
  const pivot = A[hi-1]
  let i = lo
  let j = hi-1
  while(i!==j){
    if(A[i]<=pivot){
      i++
    }else{
      swap(A,i,--j)
    }
  }
  swap(A,j,hi-1)
  return j;
}
function quick_sort(A,lo=0,hi=A.length){
  if(hi-lo<=1) return;
  const p = partition(A,lo,hi)
  quick_sort(A,lo,p)
  quick_sort(A,p+1,hi)

}
```


### 计数排序

```javascript
/*
A:数组
返回值： 无
*/

function counting_sort(A){
  const max = Math.max(...A)
  const B = Array(max+1).fill(0)
  const C = Array(A.length)
  for(let i = 0;i<A.length;i++){
    B[A[i]]++
  }
  for(let i=1;i<B.length;i++){
    B[i]+=B[i-1]
  }
  for(let i = 0;i<A.length;i++){
    const p = B[A[i]] -1
    B[A[i]]--
    C[p] = A[i]
  }
  return C
}
```

### 基数排序

```js
/*
*/
function radix_sort(A){
  const max = Math.max(...A)
  const buckets = Array.from({length:10},()=>[])
  let m = 1
  while(m<max){
    A.forEach(number=> {
      const digit = ~~((number%(m*10))/m)
      buckets[digit].push(number)
    })

    let j = 0
    buckets.forEach(bucket=>{
      while(bucket.length>0){
        A[j++] = bucket.shift()
      }
    })
    m *= 10
  }
}
```

### 桶排序

```js
/*
A:数组
K:桶的数量
S:每个桶中的数量
*/
function bucket_sort(A,k,s){
  const buckets = Array.from({length:k},()=>[])

  for(let i=0;i<A.length;i++){
    const index = ~~(A[i]/s)
    buckets[index].push(A[i])
  }
  for(let i =0;i<buckets.length;i++){
    insertion_sort(buckets[i])
  }
  return [].concat(...buckets)
}
```
## 算法模式

## 算法复杂度

> 复杂度是度量**指标**随着输入规模增长关系的一种分类。描述的是随着输入规模增加算法中最大的影响因子
> 时间复杂度是衡量算法`执行时间`随着`输入规模`增加而增长的关系,是一种对算法的分类
> 空间复杂度是指算法用了多少额外的时间

> BIG-O的本质是一种渐进趋势的描述
数学上O(n)是指随着规模增长，算法的执行时间会在 `T=cg(x)`内波动


 










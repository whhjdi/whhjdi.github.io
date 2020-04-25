---
title: 我知道的JS-深浅拷贝
date: 2018-04-15 8:07:49
tags: [JavaScript]
categories: ["Javascript"]
cover_img: https://ws3.sinaimg.cn/large/006tNbRwly1fy1m07rhyej30zk0k0atc.jpg
---

# 深浅拷贝
- 2019.9更新
对于简单类型的数据来说，赋值就是深拷贝
对于复杂类型的数据（对象）来说，才要区分浅拷贝和深拷贝

## 赋值

传递对象的引用而已,原始列表 a 改变，被赋值的 b 也会做相同的改变

<!-- more -->

```javascript
var a = { name: "muxue" };
var b = a;
b.name = "b";
a.name === "b"; // true
```

## 浅拷贝

拷贝父对象，不会拷贝对象的内部的子对象。即拷贝 a 里面的一级元素的内存地址，不拷贝 a 里的小列表里的元素的内存地址。所以有时候我们需要引入深拷贝来解决这个问题

Object.assign

```javascript
var a = { name: "muxue", age: [18, 19] };
var b = Object.assign({}, a);
b.name = "b";
b.age[0] = 20;
a.name === "b"; //false
a.age[0] === 20; //true
```

展开运算符(...)

```javascript
var a = { name: "muxue", age: [18, 19] };
var b = { ...a };
b.name = "b";
b.age[0] = 20;
a.name === "b"; //false
a.age[0] === 20; //true
```

## 深拷贝

通常使用 JSON.parse(JSON.stringify(object))来进行深拷贝，但是这个方法有缺陷

1. 会忽略 undefined
2. 不能序列化函数
3. 不能解决循环引用的对象

```javascript
var a = { name: "muxue", age: [18, 19] };
var b = JSON.parse(JSON.stringify(a));
b.name = "b";
b.age[0] = 20;
a.name === "b"; //false
a.age[0] === 20; //false
```

递归拷贝

```js
// 首先第一步需要判断是否是对象，所以写一个函数
function isObj(obj){
  //如果是对象就返回 true
  return typeof obj === 'object' && obj !== null
}
function deepClone(obj){
  if(!isObj(obj)) return obj
  let result = Array.isArray(obj) ? [] : {}
  for(let key in obj){
    if(Object.prototype.hasOwnProperty.call(obj,key)){
      result[key] = deepClone(obj[key])
    }
  }
  return result
}
//test
var a = {
    a1: undefined,
    a2: null,
    a3: 123,
    a4: 'muxue',
    a5: {
      b1: 'b1',
      b2: 'b2' 
    },
    a6: {
      b3: 123
    }
}
var b = deepClone(a);

a.a5 = 'a5';
a.a6.b3 = 456;

console.log(b);
//测试完就会发现基本的深拷贝就实现了
//然后还有两个问题需要解决
//1. 循环引用的问题
// 2. symbol的问题，这里需要解释一下，很多人会有疑问，symbol不是基本类型吗？为什么需要考虑它
//其实原因很简单，ES6的对象的key可以是字符串，也可以是symbol，所以需要考虑的symbol作为key的情况
```

解决循环引用
```js
//准备一个weakMap，weakMap的key就是你要拷贝的对象，我们需要判断weakMap有木有这个key，有就说明拷贝过了,直接return对应的值就好了
function isObj(obj){
  return typeof obj === 'object' && obj !== null
}
function deepClone(obj,map= new WeakMap()){
  if(!isObj(obj)) return obj
  if(map.has(obj)) return map.get(obj)
  let result = Array.isArray(obj) ? [] : {}
  map.set(obj,result)
  for(let key in obj){
    if(Object.prototype.hasOwnProperty.call(obj,key)){
      //这里也可以判断一下是否是对象来决定是否使用递归
      result[key] = deepClone(obj[key],map)
    }
  }
  return result
}
//test
var a = {
    a1: undefined,
    a2: null,
    a3: 123,
    a4: 'muxue',
    a5: {
      b1: 'b1',
      b2: 'b2' 
    },
    a6: {
      b3: 123
    }
}
a.circle = a
var b = deepClone(a);

a.a5 = 'a5';
a.a6.b3 = 456;

console.log(b);
//循环引用的问题就圆满解决了
```

关于Symbol的问题

```js
// 正常情况下你是遍历不到对象中的Symbol属性的
//所以一个思路是单独处理Symbol
//通过Object.getOwnPropertySymbols(obj)可以获取到对象的Symbol属性
function deepClone(obj,map= new WeakMap()){
  if(!isObj(obj)) return obj
  if(map.has(obj)) return map.get(obj)
  let result = Array.isArray(obj) ? [] : {}
  map.set(obj,result)

  let symKeys = Object.getOwnPropertySymbols(obj); // 查找
    if (symKeys.length) { // 查找成功	
        symKeys.forEach(symKey => {
            if (isObj(obj[symKey])) {
                result[symKey] = cloneDeep4(obj[symKey], map); 
            } else {
                result[symKey] = obj[symKey];
            }    
        });
    }
  for(let key in obj){
    if(Object.prototype.hasOwnProperty.call(obj,key)){
      //这里也可以判断一下是否是对象来决定是否使用递归
      if(isObj(obj[key])){
        result[key] = deepClone(obj[key],map)
      }else{
        result[key] = obj[key]
      }
    }
  }
  return result
}
//上面的方法把Symbol属性单独处理，显得不够优雅
//所以我们还可以通过反射Reflect来直接获取对象的所有属性
function deepClone(obj,map= new WeakMap()){
  if(!isObj(obj)) return obj
  if(map.has(obj)) return map.get(obj)
  let result = Array.isArray(obj) ? [] : {}
  map.set(obj,result)

  Reflect.ownKeys(obj).forEach(key=>{
    if(isObj(obj[key])){
        result[key] = deepClone(obj[key],map)
      }else{
        result[key] = obj[key]
      }
  })
  return result
}
//这种方式比较优雅

//test
var a = {
    a1: undefined,
    a2: null,
    a3: 123,
    a4: 'muxue',
    a5: {
      b1: 'b1',
      b2: 'b2' 
    },
    a6: {
      b3: 123
    },
    [Symbol('q')]:1,
    [Symbol('2')]:2
}
console.log(Object.getOwnPropertySymbols(a))
console.log(Reflect.ownKeys(a))
var b = deepClone(a);
console.log(Object.getOwnPropertySymbols(b))
console.log(Reflect.ownKeys(b))
a.a5 = 'a5';
a.a6.b3 = 456;
console.log(a)
console.log(b);
```
循环的方式
```js
// 一般来说能使用递归的也能使用循环解决,简单写一下
function deepClone(obj){
  let root = {}
  let stack = [
    {
      parent: root,
      key:undefined,
      data: obj
    }
  ]

  while(stack.length){
    let o = stack.pop()
    let parent = o.parent
    let key = o.key
    let data = o.data

    let result = root
    if(typeof key !== 'undefined'){
      result = parent[key] = {}
    }

    for( let key in data){
      if(data.hasOwnProperty(key)){
        if(isObj(data[key])){
          stack.push({
            parent: result,
            key,
            data: data[key]
          })
        }else{
          result[key] = data[key]
        }
      }
    }
  }
  return root
}
```


## 完整的实现

```javascript
function isObj(obj) {
	return typeof obj === "object" && obj !== null;
}

function isType(obj, type) {
	if (!isObj(obj)) return false;
	const typeStr = Object.prototype.toString.call(obj);
	let flag;
	switch (type) {
		case "Object":
			flag = typeStr === "[object Object]";
			break;
		case "Array":
			flag = typeStr === "[object Array]";
			break;
		case "Date":
			flag = typeStr === "[object Date]";
			break;
		case "RegExp":
			flag = typeStr === "[object RegExp]";
			break;
		default:
			flag = false;
	}
	return flag;
}

const getRegExp = re => {
  let flags = "";
  if (re.global) flags += "g";
	if (re.ignoreCase) flags += "i";
	if (re.multiline) flags += "m";
	return flags;
};

function cloneDeep(obj, hash = new WeakMap()) {
	if (!isObj(obj)) return obj;
  if (hash.has(obj)) return hash.get(obj);

	let target;
	//判断对象的类型
	if (isType(obj, "Object")) {
		let proto = Object.getPrototypeOf(obj);
		target = Object.create(proto);
	} else if (isType(obj, "Array")) {
		target = [];
	} else if (isType(obj, "Date")) {
		target = new Date(obj.getTime());
	} else if (isType(obj, "RegExp")) {
		target = new RegExp(obj.source, getRegExp(obj));
	}
	hash.set(obj, target);
	Reflect.ownKeys(obj).forEach(key => {
		if (isObj(obj[key])) {
			target[key] = cloneDeep(obj[key], hash);
		} else {
			target[key] = obj[key];
		}
	});
	return target;
}
```


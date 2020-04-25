---
title: Node快速上手
date: 2019-03-31 10:28:00
tags: [Node]
cover_img: https://ws1.sinaimg.cn/large/006tKfTcly1g1lrun4j2oj31kw0sqgqf.jpg
feature_img:
description:
keywords: [Node]
---

# Node

> Node.js 是一个开源跨平台的运行环境，用来在浏览器外执行JavaScript代码
>
> Node是创建高拓展性，数据密集型和实时的后端服务的理想型 工具

##  起源 

2009年之前，浏览器是运行js的唯一环境，直到Ryan Dahl使用V8引擎，用c++实现了Node

无阻塞，异步的特性使得Node具有很高的拓展性

## global

不同于浏览器的全局的window,在Node中global是全局对象

```javascript
CommonJS

Buffer,process,console

timer:
	setTimeout(),clearTimeout(),setInterval(),clearInterval(),setImmetiate()
```



## process

```js
argv,argv0,execArgv,execPath,cwd
```

- setImmediate和process.nextTick的区别

  nextTick要早一点（放在当前队列的队尾），setImmediate放在下一次循环的首部，事件队列的知识，在后边再写

### Buffer

用于处理二进制数据流

实例类似整数数组，大小固定

```js
const buf1 = Buffer.alloc(10);
const buf2 = Buffer.alloc(10, 0x1);
const buf3 = Buffer.alloc(5, 1);
const buf4 = Buffer.allocUnsafe(5, 1);
const buf5 = Buffer.from([1, 2, 3]);
const buf6 = Buffer.from("test");
const buf7 = Buffer.from("test", "base64");
console.log(buf1, buf2, buf3, buf4, buf5, buf6, buf7);
console.log(Buffer.byteLength(buf1));
console.log(Buffer.isBuffer(buf1));
const buf8 = Buffer.concat([buf6, buf7]);
console.log(buf8.toString());
//实例的方法
buf.length
buf8.toString('base64')
buf.fill(10,2,6)
buf.equals()
buf.indexOf()
buf.copy()
```

```

```



### 模块系统



声明的变量并不添加到global对象中，而是在当前文件中，每一个文件都可以看作一个模块。所以我们需要一种导入导出模块的机制


```javascript
function log(){
  console.log('log')
}
//1. 通过对象的形式导出
module.exports.log  = log 
//在另一个文件中就可以通过 require引入来调用
const logger = require('路径')
logger.log()

//2. 直接导出
module.exports = log 
//在另一个文件中就可以通过 require引入来调用
const logger = require('路径')
logger()
```
实际上，Node总是用一个包装函数来包裹模块中的代码
```javascript
(function (exports, require, module, __filename, __dirname) {
	//....
})
//exports可以看做module.exports的简写，可以添加属性，但是不能更改他的指向
```

### path模块

```js
reslove,join,normalize,
basename,extname,dirname,
parse,format
sep,delimiter,
```

`__dirname`,` __filename`总是返回绝对路径

process.cwd返回node命令所在的 文件夹



```javascript
const path = require('path');
var pathObj = path.parse(__dirname)
var pathObj2 = path.parse(__filename)
console.log(pathObj, pathObj2)


{ root: '/',
  dir: '/Users/muxue/Desktop',
  base: 'learn-node',
  ext: '',
  name: 'learn-node' 
} 
{ root: '/',
  dir: '/Users/muxue/Desktop/learn-node',
  base: 'app.js',
  ext: '.js',
  name: 'app' 
}
```

### stream

```js
const fs = require("fs");

//读
// const rs = fs.createReadStream("./app.js");

// rs.pipe(process.stdout);


//写
const ws = fs.createWriteStream("./text.txt");
const tid = setInterval(() => {
	const num = parseInt(Math.random() * 10);

	if (num < 8) {
		ws.write(num + "");
	} else {
		clearInterval(tid);
		ws.end();
	}
}, 200);

ws.on("finish", () => {
	console.log("done");
});

```

- 关于回调地狱

```js
const fs = require("fs");

const promisify = require("util").promisify;

const read = promisify(fs.readFile);

//使用promise
// read("./app.js")
// 	.then(data => {
// 		console.log(data.toString());
// 	})
// 	.catch(err => {
// 		console.log(err);
// 	});

//使用async/await
async function test() {
	try {
		const data = await read("./app.js");
		console.log(data.toString());
	} catch (err) {
		console.log(err);
	}
}

test();

```

### 



### OS模块

```javascript
const os = require('os')

var totalmem = os.totalmem()
var freemem = os.freemem()
console.log('totalmem: ' + totalmem, );
console.log('freemem: ' + freemem, );

totalmem: 8589934592
freemem: 414740480
```

### fs模块

```javascript
const fs = require('fs')
//同步
const files = fs.readdirSync('./')
console.log(files);
//异步
fs.readdir('./', (err, files) => {
  if (err) {
    console.log(err);
  } else {
    console.log(files);
  }
})

[ 'app.js', 'logger.js' ]
```

### events模块

```javacript
const EventEmitter = require('events')

const emitter = new EventEmitter()
emitter.on('messageLogged', (e) => {
  console.log(e);

})
emitter.emit('messageLogged', {
  id: 1,
  name: 'muxue'
})
```

```javascript
//app.js
const Logger = require('./logger')
const logger = new Logger()

logger.on('messageLogged', (e) => {
  console.log(e);
})

logger.log('message');

//logger.js
const EventEmitter = require('events')
class Logger extends EventEmitter {
  log(msg) {
    console.log(msg);
    this.emit('messageLogged', {
      id: 1,
      name: 'muxue'
    })
  }
}

module.exports = Logger
```



### http模块

```javascript
const http = require('http');

const server = http.createServer(function (req, res) {
  if (req.url === '/') {
    res.write('hello world')
    res.end();
  }
  if (req.url === '/api/name') {
    res.write(JSON.stringify([1, 2, 3]))
    res.end();
  }
})

server.listen(3000,()=>{
	console.log('Listening on port 3000')
})

```





## debug

### Inspector调试

### IDE调试



## 项目配置

gitignore,editconfig,ESLint




































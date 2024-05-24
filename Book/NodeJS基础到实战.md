## nodejs 概述

- nodejs 是异步事件驱动的 <strong>js 运行环境</strong>，并非一门编程语言。
- nodejs 底层采用 <strong>event-loop 模型</strong>实现非阻塞。

序列化 API

---

GC garbage collection 垃圾回收

![alt text](./NodeJS基础到实战/image-7.png)

---

### buffer

```js
// encoding - 使用的编码。默认为 'utf8' 。

// 写入缓冲区
const buf = Buffer.from("Friday"); // 英文一个字符对应一个字节
const buf2 = Buffer.from("周五"); // 中文一个字符对应三个字节
console.log("buf", buf); // <Buffer 46 72 69 64 61 79>
console.log("buf2", buf2); // <Buffer e5 91 a8 e4 ba 94>

// 从缓冲区读取数据
const str = buf.toString();
console.log("str", str); // Friday

// 缓冲区合并
var buffer1 = Buffer.from("百度");
var buffer2 = Buffer.from("www.baidu.com");
var buffer3 = Buffer.concat([buffer1, buffer2]);
console.log("buffer3 内容: " + buffer3.toString()); // 百度www.baidu.com

// 缓冲区比较
var buffer4 = Buffer.from("ABC");
var buffer4 = Buffer.from("ABCD");
var result = buffer4.compare(buffer4); // 0  表示两个缓冲区相等
console.log("result: " + result);

// 缓冲区长度
var buffer = Buffer.from("www.baidu.com");
//  缓冲区长度
console.log("buffer length: " + buffer.length); // 13
```

![alt text](./NodeJS基础到实战/image-8.png)

---

### module

通常 module.exports 抛出一个对象 export 抛出一个不是对象类型的数据类型

a.js

```js
exports.hello = function () {
  console.log("hello world");
};
```

b.js

```js
function HelloWorld() {
  let name = "World";

  this.setName = function (thename) {
    name = thename;
  };

  this.sayHello = function () {
    console.log("Hello " + name);
  };
}

module.exports = HelloWorld;
```

index.js

```js
let hello = require("./a");

let HelloWorld = require("./b");

let h2 = new HelloWorld();

hello.hello();

h2.setName("James");
h2.sayHello();
```

![alt text](./NodeJS基础到实战/image-9.png)

---

### nodejs process.nextick setimmate

在 Node.js 中，process.nextTick() 和 setImmediate() 是用来调度事件循环中的回调函数的方法。它们的主要区别在于执行的优先级：process.nextTick() 会在当前执行栈的尾端执行，而 setImmediate() 会在下一个事件循环迭代时立即执行。

以下是一个简单的例子，展示了如何使用 process.nextTick() 和 setImmediate()：

```js
console.log("Script Started");

process.nextTick(() => {
  console.log("NextTick Callback 1");
});

setImmediate(() => {
  console.log("Immediate Callback 1");
});

process.nextTick(() => {
  console.log("NextTick Callback 2");
});

setImmediate(() => {
  console.log("Immediate Callback 2");
});

console.log("Script Ended");
```

```txt
Script Started
Script Ended
NextTick Callback 1
NextTick Callback 2
Immediate Callback 1
Immediate Callback 2
```

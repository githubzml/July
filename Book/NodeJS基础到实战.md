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

### stream

```js
// 从流中读取数据
let fs = require("fs");
let data = "";
// 创建一个可读流
let readerStream = fs.createReadStream("input.txt");
// 设置编码为 utf-8
readerStream.setEncoding("UTF8");
// 处理流事件 data end error
readerStream.on("data", function (chunk) {
  data += chunk;
});

readerStream.on("end", function () {
  console.log(data);
});

readerStream.on("error", function (err) {
  console.log(err.stack);
});

console.log("程序执行完毕");
```

```js
// 写入数据
let fs = require("fs");
let data =
  "小满已过，田野里麦浪滚滚，一片金黄。眼下，多地小麦陆续进入收获期，农民抢抓农时，开镰收割，田间地头到处是金色的丰收场景";

// 创建一个写入的流 写入到文件 output.txt 中
let writeStream = fs.createWriteStream("./output.txt");
// 使用 utf8 编码写入数据
writeStream.write(data, "UTF8");
// 标记文件末尾
writeStream.end();
// 处理事件流 finish error
writeStream.on("finish", () => {
  console.log("写入完成");
});

writeStream.on("error", (err) => {
  console.log(err);
});

console.log("程序执行完毕");
```

### modules

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

### file

同步读取文件

```js
let fs = require("fs");

let data = fs.readFileSync("input.txt");

console.log("data", data.toString());

console.log("Program Ended");
```

同步写入文件

```js
const fs = require("fs");
const data = "hello txt";
const write = fs.writeFileSync("output.txt", data);
console.log("程序执行完毕。");
```

### path

path.join([path1][, path2][, ...])

用于连接路径。该方法的主要用途在于，会正确使用当前系统的路径分隔符，Unix 系统是"/"，Windows 系统是"/\"。

```js
let aa = path.join("/abc/bcd", "eee", "fff");

console.log(aa); // \abc\bcd\eee\fff
```

path.resolve([from ...], to)

将 to 参数解析为绝对路径，给定的路径的序列是从右往左被处理的，
后面每个 path 被依次解析，直到构造完成一个绝对路径。
例如，给定的路径片段的序列为：/foo、/bar、baz，
则调用 path.resolve('/foo', '/bar', 'baz') 会返回 /bar/baz。

```js
let bb = path.resolve("wwwroot", "static_files/png/", "../gif/image.gif");

console.log(bb); // C:\zml\zml2024learn\nodeLearn\path\wwwroot\static_files\gif\image.gif

let cc = path.resolve("foo", "bar", "baz");

console.log(cc); // C:\zml\zml2024learn\nodeLearn\path\foo\bar\baz

let ee = path.resolve("/foo/bar", "./baz");

console.log(ee); // C:\foo\bar\baz
```

path.extname(p)

返回路径中文件的后缀名，即路径中最后一个'.'之后的部分。如果一个路径中并不包含'.'或该路径只包含一个'.' 且这个'.'为路径的第一个字符，则此命令返回空字符串

```js
// extname 路径中文件的后缀名
console.log("ext name : " + path.extname("main.js")); // ext name : .js
```

### os

```js
const os = require("os");
console.log(os.hostname()); // 返回操作系统的主机名。
console.log(os.type()); // 返回操作系统名
console.log(os.platform()); // 返回编译时的操作系统名
console.log(os.arch()); // 返回操作系统 CPU 架构，可能的值有 "x64"、"arm" 和 "ia32"。
console.log(os.release()); // 返回操作系统的发行版本
console.log(os.uptime()); // 返回操作系统运行的时间，以秒为单位。
console.log(os.cpus()); // 返回一个对象数组，包含所安装的每个 CPU/内核的信息：型号、速度（单位 MHz）、时间（一个包含 user、nice、sys、idle 和 irq 所使用 CPU/内核毫秒数的对象）。
```

### quertystring

[querystring](https://nodejs.p2hp.com/api/v19/querystring/)

- querystring.decode()
- querystring.parse()

The querystring.decode() function is an alias for querystring.parse().

```js
let str = "foo=bar&abc=xyz&abc=123";
console.log(querystring.parse(str)); // 反序列化
console.log(querystring.decode(str)); // 反序列化
```

- querystring.encode()
- querystring.stringify()

The querystring.encode() function is an alias for querystring.stringify().

```js
let str2 = {
  foo: "bar",
  abc: ["xyz", "123"],
};

console.log(querystring.stringify(str2)); // 序列化
console.log(querystring.encode(str2)); // 序列化
```

- querystring.escape(str)
- querystring.unescape(str)

```js
// escape可使传入的字符串进行编码
console.log(querystring.escape("name=慕白"));

// unescape方法可将含有%的字符串进行解码
console.log(querystring.unescape("name%3D%E6%85%95%E7%99%BD"));
```

### String Decoder

```js
const { StringDecoder } = require("node:string_decoder");
const decoder = new StringDecoder("utf8");

decoder.write(Buffer.from([0xe2]));
decoder.write(Buffer.from([0x82]));
console.log(decoder.end(Buffer.from([0xac])));
```

### crypto

MD5 解密依靠彩虹表（即数据库里是否存在这样的映射关系）

1. hash 函数

```js
const { createHash } = require("crypto");

const hash = createHash("sha256");

hash.write("some data to hash");
hash.end();

console.log(hash.copy().digest("hex"));
```

2. 消息认证：粗略理解为加盐 （加一些任意随机字符串）

```cjs
const { createHmac } = require("node:crypto");

const hmac = createHmac("sha256", "a secret");

hmac.on("readable", () => {
  // Only one element is going to be produced by the
  // hash stream.
  const data = hmac.read();
  if (data) {
    console.log(data.toString("hex"));
    // Prints:
    //   7fd04df92f636fd450bc841c9418e5825c17f33ad9c87c518115a45971f7f77e
  }
});

hmac.write("some data to hash");
hmac.end();
```

3. 加密是信息被加密

对称--加密

```js
const { scryptSync, createCipheriv } = require("node:crypto");

const algorithm = "aes-192-cbc";
const password = "Password used to generate key";
const key = scryptSync(password, "salt", 24);
const iv = Buffer.alloc(16, 0);
console.log("iv", iv);

// Once we have the key and iv, we can create and use the cipher...
const cipher = createCipheriv(algorithm, key, iv);

let encrypted = "";
cipher.setEncoding("hex");

cipher.on("data", (chunk) => (encrypted += chunk));
cipher.on("end", () => console.log(123, encrypted));

cipher.write("some clear text data");
cipher.end();
```

对称--解密

```js
const { scryptSync, createDecipheriv } = require("node:crypto");
const { Buffer } = require("node:buffer");

const algorithm = "aes-192-cbc";
const password = "Password used to generate key";
// Key length is dependent on the algorithm. In this case for aes192, it is
// 24 bytes (192 bits).
// Use the async `crypto.scrypt()` instead.
const key = scryptSync(password, "salt", 24);
// The IV is usually passed along with the ciphertext.
const iv = Buffer.alloc(16, 0); // Initialization vector.

const decipher = createDecipheriv(algorithm, key, iv);

let decrypted = "";
decipher.on("readable", () => {
  let chunk;
  while (null !== (chunk = decipher.read())) {
    decrypted += chunk.toString("utf8");
  }
});
decipher.on("end", () => {
  console.log(234, decrypted);
  // Prints: some clear text data
});

// Encrypted with same algorithm, key and iv.
const encrypted =
  "e5f79c5915c02171eec6b212d5520d44480993d7d622a7c4c2da32f6efda0ffa";
decipher.write(encrypted, "hex");
decipher.end();
```

4. 非对称加密--DiffieHellman

```js
const { createDiffieHellman } = require("node:crypto");

const DiffieHellman = createDiffieHellman(512);

DiffieHellman.generateKeys();

const localPubilcKey = DiffieHellman.getPublicKey("hex");
const localPrivateKey = DiffieHellman.getPrivateKey("hex");

console.log("公钥：", localPubilcKey);
console.log("私钥：", localPrivateKey);
```

非对称加密--ECDH

```js
const crypto = require("crypto");
const ecdh = crypto.createECDH("secp256k1");
ecdh.generateKeys();
const localPubilcKey = ecdh.getPublicKey("hex");
const localPrivateKey = ecdh.getPrivateKey("hex");

console.log("公钥：", localPubilcKey);
console.log("私钥：", localPrivateKey);
```

5. 防止信息被篡改 签名认证 加签

```js
const {
  generateKeyPairSync,
  createSign,
  createVerify,
} = require("node:crypto");

const { privateKey, publicKey } = generateKeyPairSync("ec", {
  namedCurve: "sect239k1",
});

const sign = createSign("SHA256");
sign.write("some data to sign");
sign.end();
const signature = sign.sign(privateKey, "hex");

const verify = createVerify("SHA256");
verify.write("some data to sign");
verify.end();
console.log(verify.verify(publicKey, signature, "hex"));
// Prints: true
```

### zlib

```js
const fs = require("fs");
const zlib = require("zlib");

// 压缩
function Gzip(in_path, out_path) {
  const gzip = zlib.createGzip();
  const rs = fs.createReadStream(in_path);
  const ws = fs.createWriteStream(out_path);
  rs.pipe(gzip).pipe(ws);
}

// Gzip("./data/cleanCode.pdf", "./data/cleanCode.pdf.gz");

// 解压

function unZip(in_path, out_path) {
  const gzip = zlib.createGunzip();
  const rs = fs.createReadStream(in_path);
  const ws = fs.createWriteStream(out_path);
  rs.pipe(gzip).pipe(ws);
}

unZip("./data/cleanCode.pdf.gz", "./data/cc.pdf");
```

发起请求的压缩

client.js

```js
// client request example
const zlib = require("node:zlib");
const http = require("node:http");
const fs = require("node:fs");
const { pipeline } = require("node:stream");

const request = http.get({
  host: "localhost",
  path: "/",
  port: 1337,
  headers: { "Accept-Encoding": "br,gzip,deflate" },
});
request.on("response", (response) => {
  const output = fs.createWriteStream("example.com_index.html");

  const onError = (err) => {
    if (err) {
      console.error("An error occurred:", err);
      process.exitCode = 1;
    }
  };
  console.log("=========111>", response.headers["content-encoding"]);
  switch (response.headers["content-encoding"]) {
    case "br":
      pipeline(response, zlib.createBrotliDecompress(), output, onError);
      break;
    // Or, just use zlib.createUnzip() to handle both of the following cases:
    case "gzip":
      pipeline(response, zlib.createGunzip(), output, onError);
      break;
    case "deflate":
      pipeline(response, zlib.createInflate(), output, onError);
      break;
    default:
      pipeline(response, output, onError);
      break;
  }
});
```

server.js

```js
// server example
// Running a gzip operation on every request is quite expensive.
// It would be much more efficient to cache the compressed buffer.
const zlib = require("node:zlib");
const http = require("node:http");
const fs = require("node:fs");
const { pipeline } = require("node:stream");

http
  .createServer((request, response) => {
    const raw = fs.createReadStream("./data/index.html");
    // Store both a compressed and an uncompressed version of the resource.
    response.setHeader("Vary", "Accept-Encoding");
    let acceptEncoding = request.headers["accept-encoding"];
    if (!acceptEncoding) {
      acceptEncoding = "";
    }

    const onError = (err) => {
      if (err) {
        // If an error occurs, there's not much we can do because
        // the server has already sent the 200 response code and
        // some amount of data has already been sent to the client.
        // The best we can do is terminate the response immediately
        // and log the error.
        response.end();
        console.error("An error occurred:", err);
      }
    };

    // Note: This is not a conformant accept-encoding parser.
    // See https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.3
    if (/\bdeflate\b/.test(acceptEncoding)) {
      response.writeHead(200, { "Content-Encoding": "deflate" });
      pipeline(raw, zlib.createDeflate(), response, onError);
    } else if (/\bgzip\b/.test(acceptEncoding)) {
      response.writeHead(200, { "Content-Encoding": "gzip" });
      pipeline(raw, zlib.createGzip(), response, onError);
    } else if (/\bbr\b/.test(acceptEncoding)) {
      response.writeHead(200, { "Content-Encoding": "br" });
      pipeline(raw, zlib.createBrotliCompress(), response, onError);
    } else {
      response.writeHead(200, {});
      pipeline(raw, response, onError);
    }
  })
  .listen(1337);
```

index.html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <h2>今天周六！！！</h2>
  </body>
</html>
```

![alt text](NodeJS基础到实战/assets/image.png)

### 工具模块

#### util.callbackify

```js
const util = require("util");

async function fn() {
  return "hello world";
}
const callbackFunction = util.callbackify(fn);

callbackFunction((err, ret) => {
  if (err) throw err;
  console.log(ret);
});
```

#### util.promisify(original)

```js
const util = require("node:util");
const fs = require("node:fs");

const stat = util.promisify(fs.stat);
stat(".")
  .then((stats) => {
    // Do something with `stats`
    console.log(123);
  })
  .catch((error) => {
    // Handle the error.
  });
```

```js
const util = require("node:util");
const fs = require("node:fs");

const stat = util.promisify(fs.stat);

async function callStat() {
  const stats = await stat(".");
  console.log(`This directory is owned by ${stats.uid}`);
}
```

#### util.deprecate(fn, msg[, code])

```js
const util = require("node:util");

exports.obsoleteFunction = util.deprecate(() => {
  // Do something here.
}, "obsoleteFunction() is deprecated. Use newShinyFunction() instead.");
```

#### util.types.isDate(value)

```js
util.types.isDate(new Date());
```

### Readline 逐行读取内容

```js
const fs = require("node:fs");
const readline = require("node:readline");

const rl = readline.createInterface({
  input: fs.createReadStream("sample.txt"),
  crlfDelay: Infinity,
});

rl.on("line", (line) => {
  console.log(`Line from file: ${line}`);
});
```

```js
const readline = require("node:readline");
const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
  prompt: "OHAI> ",
});

rl.prompt();

rl.on("line", (line) => {
  switch (line.trim()) {
    case "hello":
      console.log("world!");
      break;
    default:
      console.log(`Say what? I might have heard '${line.trim()}'`);
      break;
  }
  rl.prompt();
}).on("close", () => {
  console.log("Have a great day!");
  process.exit(0);
});
```

### Internationalization Support

### repl

```js
const repl = require("node:repl");

const replServer = repl.start({ prompt: "> " });
replServer.defineCommand("sayhello", {
  help: "Say hello",
  action(name) {
    this.clearBufferedCommand();
    console.log(`Hello, ${name}!`);
    this.displayPrompt();
  },
});
replServer.defineCommand("saybye", function saybye() {
  console.log("Goodbye!");
  this.close();
});
```

### UDP

server.js

```js
const dgram = require("node:dgram");
const server = dgram.createSocket("udp4");

server.on("error", (err) => {
  console.log(`server error:\n${err.stack}`);
  server.close();
});

server.on("message", (msg, rinfo) => {
  console.log(`server got: ${msg} from ${rinfo.address}:${rinfo.port}`);
});

server.on("listening", () => {
  const address = server.address();
  console.log(`server listening ${address.address}:${address.port}`);
});

server.bind(41234);
// Prints: server listening 0.0.0.0:41234
```

client.js

```js
const dgram = require("node:dgram");

const client = dgram.createSocket("udp4");

client.on("error", (err) => {
  console.log(`client error:\n${err.stack}`);
  client.close();
});

setInterval(() => {
  client.send("Hello sunday", 41234, "localhost", (err, bytes) => {
    // client.close();
    console.log(`客户端发送了 ${bytes} 字节`);
  });
}, 1000);
```

### TCP

server.js

```js
const net = require("node:net");
const server = net.createServer((c) => {
  c.write("from server!!!");

  c.on("data", (data) => {
    console.log("来自客户端的数据~", data.toString());
  });

  c.on("end", () => {
    console.log("server close!!!");
  });
});

server.on("error", (err) => {
  throw err;
});

server.listen(8124, () => {
  console.log("server listen on 8124");
});
```

client.js

```js
const net = require("node:net");
const client = net.connect({ port: 8124 }, () => {
  client.write("from client!!!");

  client.on("data", (data) => {
    console.log("来自服务端的数据", data.toString());

    client.end();
  });
});

client.on("error", (err) => {
  throw err;
});

client.on("end", () => {
  console.log("client close!!!");
});
```

### url

```js
const url = require("url");

const myURL = new URL(
  "https://user:pass@sub.example.com:8080/p/a/t/h?query=string#hash"
);

console.log(myURL.protocol);
console.log(myURL.username);
console.log(myURL.hostname);
console.log(myURL.pathname);
console.log(myURL.search); // 参数
console.log(myURL.searchParams.get("query"));
console.log(myURL.hash);
console.log(url.urlToHttpOptions(myURL)); // http 协议所有参数
```

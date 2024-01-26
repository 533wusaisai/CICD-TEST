Node.js 核心模块是 Node.js 自带的模块，不需要额外安装，可以直接通过 require() 函数引入。以下是 Node.js 核心模块的列表和简要说明，以及基本的使用示例：

assert - 用于执行各种断言测试。

Copy
const assert = require('assert');
assert.strictEqual(1, 1); // 通过


buffer - 用于处理二进制数据。

Copy
const buffer = Buffer.from('Node.js');
console.log(buffer.toString()); // 输出: Node.js


child_process - 用于创建子进程。

Copy
const { exec } = require('child_process');
exec('ls', (error, stdout, stderr) => {
  if (error) {
    console.error(`执行的错误: ${error}`);
    return;
  }
  console.log(`stdout: ${stdout}`);
  console.error(`stderr: ${stderr}`);
});


cluster - 用于允许 Node.js 应用程序运行多个进程。

Copy
const cluster = require('cluster');
const http = require('http');
const numCPUs = require('os').cpus().length;

if (cluster.isMaster) {
  console.log(`主进程 ${process.pid} 正在运行`);

  // 衍生工作进程。
  for (let i = 0; i < numCPUs; i++) {
    cluster.fork();
  }

  cluster.on('exit', (worker, code, signal) => {
    console.log(`工作进程 ${worker.process.pid} 已退出`);
  });
} else {
  // 工作进程可以共享任何 TCP 连接。
  // 在本例子中，它是一个 HTTP 服务器。
  http.createServer((req, res) => {
    res.writeHead(200);
    res.end('你好世界\n');
  }).listen(8000);

  console.log(`工作进程 ${process.pid} 已启动`);
}


crypto - 提供加密功能，包括对 OpenSSL 的封装。

Copy
const crypto = require('crypto');
const secret = 'abcdefg';
const hash = crypto.createHmac('sha256', secret)
                   .update('Node.js')
                   .digest('hex');
console.log(hash);


dgram - 提供了 UDP 数据包 socket 的实现。

Copy
const dgram = require('dgram');
const server = dgram.createSocket('udp4');

server.on('error', (err) => {
  console.log(`服务器异常：\n${err.stack}`);
  server.close();
});

server.on('message', (msg, rinfo) => {
  console.log(`服务器收到来自 ${rinfo.address}:${rinfo.port} 的 ${msg}`);
});

server.on('listening', () => {
  const address = server.address();
  console.log(`服务器监听 ${address.address}:${address.port}`);
});

server.bind(41234);
// 服务器监听 0.0.0.0:41234


dns - 用于进行 DNS 查询。

Copy
const dns = require('dns');
dns.lookup('nodejs.org', (err, address, family) => {
  console.log('地址: %j 地址族: IPv%s', address, family);
});


events - 用于事件发射和监听。

Copy
const EventEmitter = require('events');
class MyEmitter extends EventEmitter {}
const myEmitter = new MyEmitter();
myEmitter.on('event', () => {
  console.log('触发了一个事件！');
});
myEmitter.emit('event');


fs - 文件系统操作。

Copy
const fs = require('fs');
fs.readFile('/path/to/file', (err, data) => {
  if (err) throw err;
  console.log(data);
});


http - HTTP 服务器和客户端。

Copy
const http = require('http');
const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World\n');
});
server.listen(3000, () => {
  console.log(`服务器运行在 http://localhost:3000/`);
});


https - HTTPS 服务器和客户端。

Copy
const https = require('https');
https.get('https://encrypted.google.com/', (res) => {
  console.log('状态码:', res.statusCode);
  console.log('请求头:', res.headers);

  res.on('data', (d) => {
    process.stdout.write(d);
  });
}).on('error', (e) => {
  console.error(e);
});


net - 用于创建基于流的 TCP 或 IPC 的服务器（net.createServer()）和客户端（net.createConnection()）。

Copy
const net = require('net');
const client = net.createConnection({ port: 8124 }, () => {
  //'connect' 监听器
  console.log('已连接到服务器！');
  client.write('世界，你好！\r\n');
});
client.on('data', (data) => {
  console.log(data.toString());
  client.end();
});
client.on('end', () => {
  console.log('已从服务器断开');
});


os - 提供了一些基本的操作系统相关的实用函数。

Copy
const os = require('os');
console.log('操作系统平台：', os.platform());


path - 用于处理文件路径。

Copy
const path = require('path');
console.log(path.basename('/path/to/file.txt')); // 输出: file.txt


process - 提供与当前进程的信息交互以及控制当前进程的能力。

Copy
process.on('exit', (code) => {
  console.log(`即将退出，退出码：${code}`);
});


querystring - 提供了一些实用工具，用于解析与格式化 URL 查询字符串。

Copy
const querystring = require('querystring');
const qs = querystring.parse('name=Node.js&book=Professional');
console.log(qs); // 输出: { name: 'Node.js', book: 'Professional' }


readline - 用于逐行读取可读流（如 process.stdin）。

Copy
const readline = require('readline');
const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout
});
rl.question('你如何看待 Node.js？', (answer) => {
  console.log(`感谢您的宝贵意见：${answer}`);
  rl.close();
});


stream - 用于处理流数据。

Copy
const stream = require('stream');
const readable = new stream.Readable({
  read() {}
});
readable.push('hi!');
readable.push(null);
readable.on('data', (chunk) => {
  console.log(`接收到 ${chunk.length} 字节的数据。`);
});


string_decoder - 提供了一个 API，用于将 Buffer 对象解码成字符串，但会保留编码的多字节 UTF-8 和 UTF-16 字符。

Copy
const StringDecoder = require('string_decoder').StringDecoder;
const decoder = new StringDecoder('utf8');
const buf = Buffer.from('中文字符串！');
console.log(decoder.write(buf));


timers - 用于执行代码块或函数后某段时间。

Copy
const { setTimeout } = require('timers');
setTimeout(() => {
  console.log('这会在大约 1000 毫秒后执行');
}, 1000);


tls - 实现了“传输层安全协议”。

Copy
// 示例略，因为它通常用于创建安全的网络服务器和客户端。


tty - 提供了 tty.ReadStream 和 tty.WriteStream 类。通常可以通过检查 process.stdin.isTTY 和 process.stdout.isTTY 来访问这些类。

Copy
const tty = require('tty');
if (tty.isatty(0)) {
  console.log('标准输入是终端');
} else {
  console.log('标准输入不是终端');
}


url - 用于 URL 解析和格式化。

Copy
const url = require('url');
const myURL = new URL('https://example.org/foo#bar');
console.log(myURL.hash); // 输出: #bar


util - 提供了一系列实用小工具。

Copy
const util = require('util');
const debuglog = util.debuglog('foo');
debuglog('hello from foo [%d]', 123);


v8 - 暴露了 V8 引擎的一些特性。

Copy
const v8 = require('v8');
console.log(v8.getHeapStatistics());


vm - 提供了在 V8 虚拟机上下文中编译和运行代码的机制。

Copy
const vm = require('vm');
const x = 1;
const sandbox = { x: 2 };
vm.createContext(sandbox); // 上下文隔离沙箱
vm.runInContext('x += 40; var y = 17;', sandbox);
console.log(sandbox.x); // 42
console.log(sandbox.y); // 17


zlib - 提供了通过 Gzip 和 Deflate/Inflate 实现压缩功能的能力。

Copy
const zlib = require('zlib');
const gzip = zlib.createGzip();
const fs = require('fs');
const inp = fs.createReadStream('input.txt');
const out = fs.createWriteStream('input.txt.gz');
inp.pipe(gzip).pipe(out);


这些是 Node.js 的核心模块，它们提供了丰富的 API 以支持各种服务器端编程任务。

至于 Koa2，它是一个轻量级的 Node.js Web 框架，它没有自己的“核心模块”概念，但它有一些常用的中间件，这些中间件可以通过 npm 安装并在 Koa 应用程序中使用。以下是一些常用的 Koa 中间件：

koa-router - 用于路由控制。
koa-bodyparser - 用于解析请求体。
koa-static - 用于提供静态文件服务。
koa-session - 用于会话管理。
koa-views - 用于模板渲染。

每个中间件都有自己的 API 和用法，通常可以在其 npm 页面上找到详细的文档和使用示例。


---------
使用HTTPS
在 Koa2 中创建支持 HTTPS 的服务器，你需要使用 Node.js 的 https 模块，并且需要有 SSL 证书（通常是一个 .crt 文件）和私钥（通常是一个 .key 文件）。以下是一个简单的示例，展示了如何使用 Koa2 和 https 模块来创建一个 HTTPS 服务器：

首先，确保你已经安装了 Koa：

Copy
npm install koa


然后，创建一个名为 server.js 的文件，并添加以下代码：

Copy
const fs = require('fs');
const https = require('https');
const Koa = require('koa');
const app = new Koa();

// 这里是你的 Koa 中间件和路由配置
app.use(async ctx => {
  ctx.body = 'Hello World';
});

// 读取证书和私钥文件
const options = {
  key: fs.readFileSync('path/to/your/private.key'), // 私钥文件路径
  cert: fs.readFileSync('path/to/your/certificate.crt') // 证书文件路径
};

// 创建 HTTPS 服务器
https.createServer(options, app.callback()).listen(443, () => {
  console.log('Koa HTTPS server is running');
});


在上面的代码中，你需要将 'path/to/your/private.key' 和 'path/to/your/certificate.crt' 替换为你的实际私钥和证书文件的路径。

运行你的服务器：

Copy
node server.js


现在，你的 Koa 应用应该在 HTTPS 上运行了，可以通过 https://localhost 访问（假设你是在本地机器上运行它）。如果你使用的是自签名证书，浏览器可能会显示一个安全警告，你需要确认安全异常以继续。

请注意，对于生产环境，你应该从受信任的证书颁发机构（CA）获取 SSL 证书。对于开发环境，你可以使用像 Let's Encrypt 这样的服务免费获取证书，或者使用工具如 openssl 来生成自签名证书。


----------

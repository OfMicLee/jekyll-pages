---
layout: post
title: "Node Profiler"
description: "JavaScript性能调优工具"
category: "nodejs"
tags: [nodejs,javascript,debug]
---

Node Profiler — JavaScript性能调优工具
===============

##特点
- 基于Node.js开发
- 深度介入V8
- 集成Inspector

##运行时优化
V8能在运行的过程中，收集代码的执行频度，进而对哪些高频度运行的热点代码进行动态优化，
这项优化技术十分有效，它并不优化所有的代码，而是找到该优化的代码去优化，将好钢用到刀刃上。

- 原生web-inspector只给出JS函数的执行占比，未说明函数是否被优化
- Node Profiler展示执行占比、是否优化、未被优化的原因

## Bailout
- ###概念
即无法优化，或者优化失败的情况。V8针对JavaScript代码的优化是基于函数为单位的，也就是说一个函数的某段代码造成无法优化，会导致整个函数处于未优化的状态。所以即使某些Bailout情况无法避开，但可以确保它们处于一个极小的函数中，以避免整个函数无法优化。

- ###CASE
  - 带有try/catch或者try/finally语句的函数
  - 带有with语句的函数
  - 用for in来进行迭代并不是一种高效的做法，改进方式是使用Object.keys()取出keys，然后再进行普通的for循环
  - 带有yield语句的Generator函数
  - 赋值给形参中的参数
  - 其他。。。

##实测
### 使用示例
```js
var http = require('http');
http.createServer(function (req, res) {
    res.writeHead(200, {'Content-type' : 'text/plain' });
    res.write('Hi, MicLee!\n')
    res.end('hello world!');
  }).listen(2014, '127.0.0.1');
console.log('Server running at http://127.0.0.1:2014/');
```

```shell
Server running at http://127.0.0.1:2014/
webkit-devtools-agent: Spawning websocket service process...
start agent

webkit-devtools-agent: A proxy got connected.
webkit-devtools-agent: Waiting for commands...
webkit-devtools-agent: Websockets service started on 0.0.0.0:9999  <==启动成功
```
如出现如下：
```shell
Error: listen EADDRINUSE           <== 可能是由于端口被占用
```

成功启动后，自动弹出chrome（如果有安装的话）url (http://alinode.aliyun.com/profiler/inspector.html?host=localhost:9999&page=0)
出现如下界面：
![](https://cloud.githubusercontent.com/assets/3832082/8587127/7b54f88c-262a-11e5-9298-3a49c2b71d7c.jpg)

默认**Collect JavaSript CPU Profile**，单击**Start**。

可以采用压测脚本实现对服务进行压力测试，保证更多的结果：
```shell
$ab -n1000 -c10 http://localhost:2014/
```
点击**Stop**，得到如下图的结果：
![](https://cloud.githubusercontent.com/assets/3832082/8587247/8dc33cbc-262b-11e5-8a10-59c8f9de280e.jpg)

可以看到更多关于函数在运行时的信息。

### UI含义
UI 栏目 | 示意
----   | ----
Self | exclusive time
Total | inclusive time
**# of Hidden Classes** | 隐藏类个数
**Bailout** | v8中提取的最后一次去优化原因
**Function** | 函数名称 script : line
**红色表示函数未被优化， 淡绿色表示函数被V8优化过。**

### 注意事项
* 该工具目前只支持X64平台（Linux, Mac, Win)。
* 切勿部署到线上，仅供自己调试使用。

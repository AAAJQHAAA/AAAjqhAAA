---
title: Node
date: 2023-12-13 16:25:41
category: 前端
tags:
  - node
---
# nodeJS
- 中文网站：https://nodejs.cn/
- 官网：https://nodejs.org/
- Node.js 是单线程的。它基于事件驱动的架构，使用了非阻塞I/O 操作，通过事件循环机制实现异步操作，这使得 Node.js 可以高效地处理大量的并发请求。
- 虽然 Node.js 本身是单线程的，但是它可以利用事件驱动、回调函数、Promise、async/await 等方式来实现并发处理，从而高效地利用计算资源。
# 事件驱动架构
- 事件驱动架构是一种软件设计范例，它基于事件和事件处理程序的概念。
- 在事件驱动架构中，软件系统的行为在很大程度上是由外部事件的发生和处理所驱动的。
  - 当一个事件发生时，系统会调用相应的事件处理程序来进行处理。
- 在事件驱动架构中，通常会有一个事件循环来监听事件的发生，当事件发生时，会触发对应的事件处理程序。
  - 这种模型通常用于实现异步、非阻塞的系统，允许系统同时处理多个事件而不需要等待每个事件处理完毕。
- 在软件开发中，事件驱动架构通常被用于构建高性能、高并发的系统，比如 Web 服务器、消息队列等。
  - Node.js 就是一个典型的事件驱动架构的例子，它使用事件循环来处理异步 I/O 操作和事件驱动的回调函数。
# Promise 和 async/await 
- JavaScript 中用于处理异步操作的两种机制
  - Promise：用来处理异步操作的内置对象
  - async/await：用于简化异步操作的语法糖
    - async 关键字：用于声明一个异步函数，该函数将返回一个 Promise 对象，
    - await 关键字：可以等待一个 Promise 对象的解决，并返回其结果

# 概念
- 非阻塞IO，IO密集，事件驱动
- 进程：一个运行的程序
- 线程：进程内一个独立可调度的执行单元
- nodejs工作模型
    - 事件驱动主进程是单线程模型
    - IO是操作系统底层多线程调度
    - 注意：单线程并不是单进程
# global全局对象
- CommonJS：module.exports、exports
- Buffer
- console
- process
- timer

- process.nextTick(()=>{})：当前事件队列尾
- setTimeout(()=>{},0)
- setImmediate(()=>{})：下一个事件队列首 

# 调试
- Inspector
    - 1、（无用）debugger[官网调试器](https://nodejs.cn/api/debugger.html)
    - 2、`node --inspect-brk myscript.js`
        - 浏览器打开：`chrome://inspect`
        - 打断点、下一个断点、进入、跳过
- vscode
    - 打断点、调试

# 基础
- `__dirname`、`__filename`返回文件绝对路径
- `process.cwd()`返回node命令启动路径
- `./` 当前文件所在文件夹
- Buffer
    - 处理二进制数据流
    - 大小固定
    - 内存是v8堆外c++内存
- Events：事件处理
- fs：文件操作














---
title: electron
date: 2024-03-01 15:46:45
category: 前端
tags:
  - electron
---

# Electron
## 入门
- [官网快速入门](https://www.electronjs.org/zh/docs/latest/tutorial/quick-start)
## 进程间的通信
- [官网进程通信](https://www.electronjs.org/zh/docs/latest/tutorial/ipc)
- 预加载脚本-暴露方法变量给->渲染器进程
- 预加载脚本-发射处理事件给->主进程
    - 预加载脚本暴露方法、变量给渲染器调用【window.versions、versions、window.versions.ping()】
```js
const { contextBridge, ipcRenderer } = require('electron')

contextBridge.exposeInMainWorld('versions', {
  node: () => process.versions.node,
  chrome: () => process.versions.chrome,
  electron: () => process.versions.electron,
  ping: () => ipcRenderer.invoke('ping'),
  // 能暴露的不仅仅是函数，我们还可以暴露变量
})
```
- 主进程
```js
const { app, BrowserWindow, ipcMain } = require('electron')
ipcMain.handle('ping', () => 'pong')
```
- 渲染器进程到主进程
    - 不需要结果
        - `ipcRenderer.send`发射事件【预加载脚本】
        - `ipcMain.on`监听事件【主进程】
    - 从渲染器进程代码调用主进程模块并等待结果
        - `ipcRenderer.invoke`发射事件【预加载脚本】
        - `ipcMain.handle`监听事件【主进程】
- 主进程到渲染器进程
    - `webContents.send`发射事件【主进程】
    - `ipcRenderer.on`监听事件【预加载脚本】
- 渲染器进程到渲染器进程
    - 将主进程作为渲染器之间的消息代理。 这需要将消息从一个渲染器发送到主进程，然后主进程将消息转发到另一个渲染器。
    - 从主进程将一个 MessagePort 传递到两个渲染器。 这将允许在初始设置后渲染器之间直接进行通信。

# electron打包工具
- electron-builder:配置项较多，更加灵活，打包体积相对较小，同时上手难度大;
- electron-packager:配置简单易上手，但是打出来的应用程序包体积相对较大;
- electron-forge【是工具集】:内部打包工具依然是electron-packager

# 应用案例（录屏）
- 实现录屏：https://mp.weixin.qq.com/s/w8bXmcwzJjZ2WM2sclXP8A
## mac上内录blackhole、Soundflower
- 下载blackhole：https://existential.audio/blackhole/
- 下载Soundflower：https://soundflower.en.softonic.com/mac?ex=DINS-635.3
- 安装完成，进入midi设置，点击左下角「+」号，选择「创建多输出设备」。
    - 在右侧菜单中的「使用」里勾选「BlackHole」（必选）和「扬声器」/「耳机」（二选一或多选）「主设备」选择「扬声器」/「耳机」。


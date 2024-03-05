---
title: electron+vue3+vite+ts项目搭建
date: 2024-03-01 15:46:45
category: 前端
tags:
  - electron
---

# electron+vue3+vite+ts构建步骤 
## 1、创建vite+vue+ts项目 
```shell script
 % npm create vite
✔ Project name: … web-audio-api-code
✔ Select a framework: › Vue
✔ Select a variant: › TypeScript

Scaffolding project in ...

Done. Now run:

  cd web-audio-api-code
  npm install
  npm run dev
```
- 安装`npm install vue-router@4`
    - 新建`src/router/index.ts`文件，创建路由
    - 修改`src/main.ts`文件，添加路由
## 2、添加electron依赖
- 依赖：`npm install electron@"^28.0.0" -D`
- 开发：`npm install electron-devtools-installer -D`
- 打包：`npm install electron-builder -D`
- vite集成：
    - `npm install vite-plugin-electron -D`
    - `npm install vite-plugin-electron-renderer -D`
- `vite.config.ts`文件中添加配置
```ts
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
+ import electron from 'vite-plugin-electron'
+ import electronRender from 'vite-plugin-electron-renderer'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [
    vue(),
+    electron([{
+      entry: 'electron/index.ts',
+    },{
+      entry: 'electron/preload.ts',
+    }),
+   electronRender()
  ],
})
```
- 新建`electron/index.ts`【主进程】
```ts
import { app, BrowserWindow } from "electron"
import path from "path"
process.env['ELECTRON_DISABLE_SECURITY_WARNINGS'] = 'true';// 隐藏安全策略控制台警告提示

const createWindow = () => {
  const win = new BrowserWindow({
    webPreferences: {
      contextIsolation: false, // 是否开启隔离上下文
      nodeIntegration: true, // 渲染进程使用Node API
      preload: path.join(__dirname, "./preload.js"), // 需要引用js文件
    },
  })
 
  // 如果打包了，渲染index.html
  if (process.env.NODE_ENV !== 'development') {
    win.loadFile(path.join(__dirname, "./index.html"))
    win.webContents.openDevTools()
  } else {
    let url = "http://localhost:5173" // 本地启动的vue项目路径。注意：vite版本3以上使用的端口5173；版本2用的是3000
    win.loadURL(url)
    win.webContents.openDevTools()
  }
}
 
app.whenReady().then(() => {
  createWindow() // 创建窗口
  app.on("activate", () => {
    if (BrowserWindow.getAllWindows().length === 0) createWindow()
  })
})
 
// 关闭窗口
app.on("window-all-closed", () => {
  if (process.platform !== "darwin") {
    app.quit()
  }
}) 
```
- 新建`electron/preload.ts`[【预加载脚本】](https://www.electronjs.org/zh/docs/latest/tutorial/tutorial-preload)
```ts
import os from "os";
console.log("platform", os.platform());
```
- `package.json`文件中添加
    - `"main": "dist-electron/index.js",`
    - `scripts.build`命令中添加`&& electron-builder`打包
    - 删除`"type": "module",`（electron不支持）
- `tsconfig.json`文件中
    - `include`中新增`, "electron/*.ts"`
- 新建`electron-builder.json5`文件，[electron-builder打包配置](https://www.electron.build/configuration/configuration)
```json5
{
  "$schema": "https://raw.githubusercontent.com/electron-userland/electron-builder/master/packages/app-builder-lib/scheme.json",
  "appId": "YourAppID",
  "asar": true,
  "productName": "YourAppName",
  "directories": {
    "output": "release/${version}"
  },
  "files": [
    "dist",
    "dist-electron"
  ],
  "mac": {
    "target": [
      "dmg"
    ],
    "artifactName": "${productName}-Mac-${version}-Installer.${ext}"
  },
  "win": {
    "target": [
      {
        "target": "nsis",
        "arch": [
          "x64"
        ]
      }
    ],
    "artifactName": "${productName}-Windows-${version}-Setup.${ext}"
  },
  "nsis": {
    "oneClick": false,
    "perMachine": false,
    "allowToChangeInstallationDirectory": true,
    "deleteAppDataOnUninstall": false
  },
  "linux": {
    "target": [
      "AppImage"
    ],
    "artifactName": "${productName}-Linux-${version}.${ext}"
  }
}
```

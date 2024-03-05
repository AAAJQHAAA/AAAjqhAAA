---
title: electron+vue2+webpack项目搭建
date: 2024-03-01 15:46:45
category: 前端
tags:
  - electron
---

# electron+vue2+webpack项目搭建
## 1、vue创建项目
- 创建项目
  - 旧客户端
    - `npm install -g vue-cli`
    - `vue init webpack my-project`
  - 新客户端3.0以上(必需卸载上面的才能装`npm uninstall -g vue-cli`)
    - `npm install -g @vue/cli`
    - `vue create my-project`
  - 如果安装的`@vue/cli`想用老模版
    - `npm i -g @vue/cli-init`
    - `vue init webpack my-project`
## 2、添加electron依赖
- `vue add electron-builder`：会初始化项目
  - 注意：多了配置和文件
- `npm install`
- `npm run electron:serve`
  - 启动慢问题：background.js中将VUEJS_DEVTOOLS相关注释掉，不加载
## 预加载脚本配置
```text
  /**
   * __dirname 字符串指向当前正在执行脚本的路径 (在本例中，它指向你的项目的根文件夹)。
   * path.join API 将多个路径联结在一起，创建一个跨平台的路径字符串。
   */
  preload: path.join(__dirname, 'preload.js')
```
- `vue.config.js`文件中添加
```js
{
    pluginOptions: {
      electronBuilder: {
        preload: 'src/preload.js'
      }
    }
}
```

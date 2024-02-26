---
title: loader加载器
date: 2024-02-04 15:20:21
category: 前端
tags:
  - webpack
  - loader
---
# html-loader
- 将HTML文件导出为字符串。当编译器需要时，将压缩HTML字符串
- 安装：`npm install --save-dev html-loader`
- 配置：
```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.html$/i,
        loader: 'html-loader',
      },
    ],
  },
};
```
- 使用：`import html from './file.html';`



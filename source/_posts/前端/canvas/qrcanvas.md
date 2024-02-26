---
title: canvas生成二维码
date: 2024-01-31 16:58:10
category: 前端
tags:
  - canvas
  - canvas生成二维码
---
# canvas生成二维码
### 包管理

- 安装:
```sh
$ yarn add qrcanvas
# or
$ npm install qrcanvas -S
```

- 使用:
``` js
import { qrcanvas } from 'qrcanvas';

const canvas = qrcanvas({
  data: 'hello, world'
});
document.body.appendChild(canvas);
```

### CDN
``` html
<div id="qrcode"></div>

<script src="https://cdn.jsdelivr.net/npm/qrcanvas@3"></script>
```
- 使用:
``` js
const canvas = qrcanvas.qrcanvas({
  data: 'hello, world'
});
document.getElementById('qrcode').appendChild(canvas);
```


---
title: canvas
date: 2024-01-31 16:58:10
category: 前端
tags:
  - canvas
---
# canvas API简介
- https://juejin.cn/column/7077005111989387271
- API：https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API
- Canvas API提供了一个通过`JS`和`HTML`的`<canvas>`元素来绘制图形的方式。它可以用于动画、游戏画面、数据可视化、图片编辑以及实时视频处理等方面。

## Canvas教程
- https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial
- canvas坐标原点在左上角：向右是x轴正方向，向下是y轴正方向
- 判断是否支持canvas

```html
<canvas id="tutorial" width="150" height="150">
  不支持canvas
</canvas>
<script>
    const canvas = document.getElementById("tutorial");
    if (canvas.getContext) {
      const ctx = canvas.getContext("2d");
      // 支持
    } else {
      // 不支持canvas
    }
</script>
```

- demo01：设置颜色，画矩形

```html
<!doctype html>
<html lang="en-US">
  <head>
    <meta charset="UTF-8" />
    <title>Canvas experiment</title>
  </head>
  <body>
    <canvas id="canvas" width="150" height="150"></canvas>
    <script type="application/javascript">
      function draw() {
        const canvas = document.getElementById("canvas");
        if (canvas.getContext) {
          const ctx = canvas.getContext("2d");
          // 设置上下文颜色
          ctx.fillStyle = "rgb(200 0 0)";
          // 绘制填充矩形(x坐标，y坐标，宽，高)
          ctx.fillRect(10, 10, 50, 50);
          // 设置上下文颜色
          ctx.fillStyle = "rgb(0 0 200 / 50%)";
          // 绘制填充矩形(x坐标，y坐标，宽，高)
          ctx.fillRect(30, 30, 50, 50);
        }
      }
      window.addEventListener("load", draw);
    </script>
  </body>
</html>
```

- demo02：矩形填充

```html
<!doctype html>
<html lang="en-US">
  <head>
    <meta charset="UTF-8" />
    <title>Canvas experiment</title>
  </head>
  <body>
    <canvas id="canvas" width="150" height="150"></canvas>
    <script type="application/javascript">
      function draw() {
        const canvas = document.getElementById("canvas");
        if (canvas.getContext) {
          const ctx = canvas.getContext("2d");
          // 绘制填充矩形的方法(x坐标，y坐标，宽，高)【黑色】
          ctx.fillRect(25, 25, 100, 100);
          // 通过把像素设置为透明以达到擦除一个矩形区域的目的(x坐标，y坐标，宽，高)【透明】
          ctx.clearRect(45, 45, 60, 60);
          // 描绘一个起点在 (x, y)、宽度为 w、高度为 h 的矩形的方法(x坐标，y坐标，宽，高)
          ctx.strokeRect(50, 50, 50, 50);
        }
      }
      window.addEventListener("load", draw);
    </script>
  </body>
</html>
```

- demo03：路径填充

```html
<!doctype html>
<html lang="en-US">
  <head>
    <meta charset="UTF-8" />
    <title>Canvas experiment</title>
  </head>
  <body>
    <canvas id="canvas" width="150" height="150"></canvas>
    <script type="application/javascript">
      function draw() {
        const canvas = document.getElementById("canvas");
        if (canvas.getContext) {
          const ctx = canvas.getContext("2d");
          // 通过清空子路径列表开始一个新路径的方法。当你想创建一个新的路径时，调用此方法。
          ctx.beginPath();
          // 将一个新的子路径的起始点移动到 (x，y) 坐标的方法。
          ctx.moveTo(75, 50);
          // 使用直线连接子路径的终点到 x，y 坐标的方法
          ctx.lineTo(100, 75);
          // 使用直线连接子路径的终点到 x，y 坐标的方法
          ctx.lineTo(100, 25);
          // 根据当前的填充样式，填充当前或已存在的路径的方法。采取非零环绕或者奇偶环绕规则
          ctx.fill();
        }
      }
      window.addEventListener("load", draw);
    </script>
  </body>
</html>
```

- demo04：路径画线

```html
<!doctype html>
<html lang="en-US">
  <head>
    <meta charset="UTF-8" />
    <title>Canvas experiment</title>
  </head>
  <body>
    <canvas id="canvas" width="150" height="150"></canvas>
    <script type="application/javascript">
      function draw() {
        const canvas = document.getElementById("canvas");
        if (canvas.getContext) {
          const ctx = canvas.getContext("2d");
          // 通过清空子路径列表开始一个新路径的方法。当你想创建一个新的路径时，调用此方法。
          ctx.beginPath();
          // 绘制圆弧路径的方法(x,y,半径,开始弧度,结束弧度,是否逆时针方向)
          ctx.arc(75, 75, 50, 0, Math.PI * 2, true); // Outer circle
          // 将一个新的子路径的起始点移动到 (x，y) 坐标的方法。
          ctx.moveTo(110, 75);
          // 绘制圆弧路径的方法(x,y,半径,开始弧度,结束弧度,是否逆时针方向)
          ctx.arc(75, 75, 35, 0, Math.PI, false); // Mouth (clockwise)
          // 将一个新的子路径的起始点移动到 (x，y) 坐标的方法。
          ctx.moveTo(65, 65);
          ctx.arc(60, 65, 5, 0, Math.PI * 2, true); // Left eye
          // 将一个新的子路径的起始点移动到 (x，y) 坐标的方法。
          ctx.moveTo(95, 65);
          // 绘制圆弧路径的方法(x,y,半径,开始弧度,结束弧度,是否逆时针方向)
          ctx.arc(90, 65, 5, 0, Math.PI * 2, true); // Right eye
          // （绘制线）使用非零环绕规则，根据当前的画线样式，绘制当前或已经存在的路径的方法
          ctx.stroke();
        }
      }
      window.addEventListener("load", draw);
    </script>
  </body>
</html>
```

- demo05：连线和填充对比

```html
<!doctype html>
<html lang="en-US">
  <head>
    <meta charset="UTF-8" />
    <title>Canvas experiment</title>
  </head>
  <body>
    <canvas id="canvas" width="150" height="150"></canvas>
    <script type="application/javascript">
      function draw() {
        const canvas = document.getElementById("canvas");
        if (canvas.getContext) {
          const ctx = canvas.getContext("2d");

          // 通过清空子路径列表开始一个新路径的方法。当你想创建一个新的路径时，调用此方法。
          ctx.beginPath();
          // 将一个新的子路径的起始点移动到 (x，y) 坐标的方法。
          ctx.moveTo(25, 25);
          // 使用直线连接子路径的终点到 x，y 坐标的方法
          ctx.lineTo(105, 25);
          // 使用直线连接子路径的终点到 x，y 坐标的方法
          ctx.lineTo(25, 105);
          // （填充）根据当前的填充样式，填充当前或已存在的路径的方法。采取非零环绕或者奇偶环绕规则
          ctx.fill();

          // 通过清空子路径列表开始一个新路径的方法。当你想创建一个新的路径时，调用此方法。
          ctx.beginPath();
          // 将一个新的子路径的起始点移动到 (x，y) 坐标的方法。
          ctx.moveTo(125, 125);
          // 使用直线连接子路径的终点到 x，y 坐标的方法
          ctx.lineTo(125, 45);
          // 使用直线连接子路径的终点到 x，y 坐标的方法
          ctx.lineTo(45, 125);
          // 将笔点连到当前子路径起始点的方法
          ctx.closePath();
          // （绘制线）使用非零环绕规则，根据当前的画线样式，绘制当前或已经存在的路径的方法
          ctx.stroke();
        }
      }
      window.addEventListener("load", draw);
    </script>
  </body>
</html>
```

- demo06：画四行三列的圆

```html
<!doctype html>
<html lang="en-US">
  <head>
    <meta charset="UTF-8" />
    <title>Canvas experiment</title>
  </head>
  <body>
    <canvas id="canvas" width="300" height="300"></canvas>
    <script type="application/javascript">
      function draw() {
        const canvas = document.getElementById("canvas");
        if (canvas.getContext) {
          const ctx = canvas.getContext("2d");
          for (let i = 0; i < 4; i++) {
            // 四行y轴
            for (let j = 0; j < 3; j++) {
              // 三列x轴
              // 通过清空子路径列表开始一个新路径的方法。当你想创建一个新的路径时，调用此方法。
              ctx.beginPath();
              // 圆心：x轴右移50，y轴下移50
              const x = 25 + j * 50; // x coordinate
              const y = 25 + i * 50; // y coordinate
              // 半径20
              const radius = 20; // Arc radius
              // 开始弧度0
              const startAngle = 0; // Starting point on circle
              // 结束弧度，π，3/2π，2π
              const endAngle = Math.PI + (Math.PI * j) / 2; // End point on circle
              // 是否逆时针，基数逆时针，偶数顺时针
              const counterclockwise = i % 2 !== 0; // clockwise or counterclockwise
              // 绘制圆弧路径的方法(x,y,半径,开始弧度,结束弧度,是否逆时针方向)
              ctx.arc(x, y, radius, startAngle, endAngle, counterclockwise);
              // 填充or画线，前两行画线，后两行填充
              if (i > 1) {
                ctx.fill();
              } else {
                ctx.stroke();
              }
            }
          }
        }
      }
      window.addEventListener("load", draw);
    </script>
  </body>
</html>
```


- demo07：贝塞尔曲线

```html
<!doctype html>
<html lang="en-US">
  <head>
    <meta charset="UTF-8" />
    <title>Canvas experiment</title>
  </head>
  <body>
    <canvas id="canvas" width="300" height="300"></canvas>
    <script type="application/javascript">
      function draw() {
        const canvas = document.getElementById("canvas");
        if (canvas.getContext) {
          const ctx = canvas.getContext("2d");
          // 通过清空子路径列表开始一个新路径的方法。当你想创建一个新的路径时，调用此方法。
          ctx.beginPath();
          // 将一个新的子路径的起始点移动到 (x，y) 坐标的方法。
          ctx.moveTo(75, 25);
          // 新增二次贝塞尔曲线路径的方法（控制点x,控制点y,端点x,端点y）
          ctx.quadraticCurveTo(25, 25, 25, 62.5);
          ctx.quadraticCurveTo(25, 100, 50, 100);
          ctx.quadraticCurveTo(50, 120, 30, 125);
          ctx.quadraticCurveTo(60, 120, 65, 100);
          ctx.quadraticCurveTo(125, 100, 125, 62.5);
          ctx.quadraticCurveTo(125, 25, 75, 25);
          // （绘制线）使用非零环绕规则，根据当前的画线样式，绘制当前或已经存在的路径的方法
          ctx.stroke();
        }
      }
      window.addEventListener("load", draw);
    </script>
  </body>
</html>
```

- demo08：三次贝赛尔曲线

```html
<!doctype html>
<html lang="en-US">
  <head>
    <meta charset="UTF-8" />
    <title>Canvas experiment</title>
  </head>
  <body>
    <canvas id="canvas" width="300" height="300"></canvas>
    <script type="application/javascript">
      function draw() {
        const canvas = document.getElementById("canvas");
        if (canvas.getContext) {
          const ctx = canvas.getContext("2d");
          // 通过清空子路径列表开始一个新路径的方法。当你想创建一个新的路径时，调用此方法。
          ctx.beginPath();
          // 将一个新的子路径的起始点移动到 (x，y) 坐标的方法。
          ctx.moveTo(75, 40);
          // 绘制三次贝赛尔曲线路径的方法（控制点x,控制点y,控制点x2,控制点y2,端点x,端点y）
          ctx.bezierCurveTo(75, 37, 70, 25, 50, 25);
          ctx.bezierCurveTo(20, 25, 20, 62.5, 20, 62.5);
          ctx.bezierCurveTo(20, 80, 40, 102, 75, 120);
          ctx.bezierCurveTo(110, 102, 130, 80, 130, 62.5);
          ctx.bezierCurveTo(130, 62.5, 130, 25, 100, 25);
          ctx.bezierCurveTo(85, 25, 75, 37, 75, 40);
          // （填充）根据当前的填充样式，填充当前或已存在的路径的方法。采取非零环绕或者奇偶环绕规则
          ctx.fill();
        }
      }
      window.addEventListener("load", draw);
    </script>
  </body>
</html>
```

- demo09：矩形圆角，贝塞尔曲线

```html
<!doctype html>
<html lang="en-US">
  <head>
    <meta charset="UTF-8" />
    <title>Canvas experiment</title>
  </head>
  <body>
    <canvas id="canvas" width="300" height="300"></canvas>
    <script type="application/javascript">
      function draw() {
        const canvas = document.getElementById("canvas");
        if (canvas.getContext) {
          const ctx = canvas.getContext("2d");

          roundedRect(ctx, 12, 12, 150, 150, 15);
          roundedRect(ctx, 19, 19, 150, 150, 9);
          roundedRect(ctx, 53, 53, 49, 33, 10);
          roundedRect(ctx, 53, 119, 49, 16, 6);
          roundedRect(ctx, 135, 53, 49, 33, 10);
          roundedRect(ctx, 135, 119, 25, 49, 10);

          ctx.beginPath();
          // 绘制圆弧路径的方法(x,y,半径,开始弧度,结束弧度,是否逆时针方向)
          ctx.arc(37, 37, 13, Math.PI / 7, -Math.PI / 7, false);
          // 使用直线连接子路径的终点到 x，y 坐标的方法
          ctx.lineTo(31, 37);
          // （填充）根据当前的填充样式，填充当前或已存在的路径的方法。采取非零环绕或者奇偶环绕规则
          ctx.fill();

          for (let i = 0; i < 8; i++) {
            // 绘制填充矩形的方法(x坐标，y坐标，宽，高)
            ctx.fillRect(51 + i * 16, 35, 4, 4);
          }

          for (let i = 0; i < 6; i++) {
            // 绘制填充矩形的方法(x坐标，y坐标，宽，高)
            ctx.fillRect(115, 51 + i * 16, 4, 4);
          }

          for (let i = 0; i < 8; i++) {
            // 绘制填充矩形的方法(x坐标，y坐标，宽，高)
            ctx.fillRect(51 + i * 16, 99, 4, 4);
          }

          // 画身体
          ctx.beginPath();
          ctx.moveTo(83, 116);
          ctx.lineTo(83, 102);
          ctx.bezierCurveTo(83, 94, 89, 88, 97, 88);
          ctx.bezierCurveTo(105, 88, 111, 94, 111, 102);
          ctx.lineTo(111, 116);
          ctx.lineTo(106.333, 111.333);
          ctx.lineTo(101.666, 116);
          ctx.lineTo(97, 111.333);
          ctx.lineTo(92.333, 116);
          ctx.lineTo(87.666, 111.333);
          ctx.lineTo(83, 116);
          ctx.fill();

          // 画眼睛
          ctx.fillStyle = "white";
          ctx.beginPath();
          ctx.moveTo(91, 96);
          ctx.bezierCurveTo(88, 96, 87, 99, 87, 101);
          ctx.bezierCurveTo(87, 103, 88, 106, 91, 106);
          ctx.bezierCurveTo(94, 106, 95, 103, 95, 101);
          ctx.bezierCurveTo(95, 99, 94, 96, 91, 96);
          ctx.moveTo(103, 96);
          ctx.bezierCurveTo(100, 96, 99, 99, 99, 101);
          ctx.bezierCurveTo(99, 103, 100, 106, 103, 106);
          ctx.bezierCurveTo(106, 106, 107, 103, 107, 101);
          ctx.bezierCurveTo(107, 99, 106, 96, 103, 96);
          ctx.fill();

          // 画眼珠
          ctx.fillStyle = "black";
          ctx.beginPath();
          ctx.arc(101, 102, 2, 0, Math.PI * 2, true);
          ctx.fill();

          ctx.beginPath();
          ctx.arc(89, 102, 2, 0, Math.PI * 2, true);
          ctx.fill();
        }
      }
      // 绘制圆角矩形的实用函数。
      function roundedRect(ctx, x, y, width, height, radius) {
        // 通过清空子路径列表开始一个新路径的方法。当你想创建一个新的路径时，调用此方法。
        ctx.beginPath();
        // 将一个新的子路径的起始点移动到 (x，y) 坐标的方法。
        ctx.moveTo(x, y + radius);
        // 根据控制点和半径绘制圆弧路径，使用当前的描点 (前一个 moveTo 或 lineTo 等函数的止点)（控制点x,控制点y,控制点x2,控制点y2,弧度）
        ctx.arcTo(x, y + height, x + radius, y + height, radius);
        ctx.arcTo(x + width, y + height, x + width, y + height - radius, radius);
        ctx.arcTo(x + width, y, x + width - radius, y, radius);
        ctx.arcTo(x, y, x, y + radius, radius);
        // （绘制线）使用非零环绕规则，根据当前的画线样式，绘制当前或已经存在的路径的方法
        ctx.stroke();
      }
      window.addEventListener("load", draw);
    </script>
  </body>
</html>
```

- demo10：一次性画线填充

```html
<!doctype html>
<html lang="en-US">
  <head>
    <meta charset="UTF-8" />
    <title>Canvas experiment</title>
  </head>
  <body>
    <canvas id="canvas" width="300" height="300"></canvas>
    <script type="application/javascript">
      function draw() {
        const canvas = document.getElementById("canvas");
        if (canvas.getContext) {
          const ctx = canvas.getContext("2d");

          ctx.beginPath();// 重新开始

          // Outter shape clockwise ⟳
          ctx.moveTo(0, 0);// 移到（0,0）
          ctx.lineTo(150, 0);// 画直线
          ctx.lineTo(75, 129.9);// 画直线

          // Inner shape anticlockwise ↺
          ctx.moveTo(75, 20);// 移到（75,20）
          ctx.lineTo(50, 60);// 画直线
          ctx.lineTo(100, 60);// 画直线

          ctx.fill();// 填充
        }
      }
      window.addEventListener("load", draw);
    </script>
  </body>
</html>
```

- demo11：等装成Path2D对象使用

```html
<!doctype html>
<html lang="en-US">
  <head>
    <meta charset="UTF-8" />
    <title>Canvas experiment</title>
  </head>
  <body>
    <canvas id="canvas" width="300" height="300"></canvas>
    <script type="application/javascript">
      function draw() {
        const canvas = document.getElementById("canvas");
        if (canvas.getContext) {
          const ctx = canvas.getContext("2d");
          // 矩形存储为一个对象
          const rectangle = new Path2D();
          rectangle.rect(10, 10, 50, 50);
          // 圆存储为一个对象
          const circle = new Path2D();
          circle.arc(100, 35, 25, 0, 2 * Math.PI);
          // 画
          ctx.stroke(rectangle);
          ctx.fill(circle);
        }
      }
      window.addEventListener("load", draw);
    </script>
  </body>
</html>
```

## Canvas深入
- https://joshondesign.com/p/books/canvasdeepdive/toc.html

## 使用 canvas 绘制视频

## 第三方库
- [EaselJS](https://www.createjs.com/easeljs) 使制作游戏、创作类艺术和其他侧重图形项目更容易的开源 canvas 库
- [Fabric.js](http://fabricjs.com/) 具有 SVG 解析功能的开源 canvas 库
- [heatmap.js](https://www.patrick-wied.at/static/heatmapjs/) 基于 canvas 的热点图的开源库
- [JavaScript InfoVis Toolkit](https://thejit.org/) 创建交互式的 2D Canvas 数据可视化
- [Konva.js](https://konvajs.github.io/) 用于桌面端和移动端应用的 2D canvas 库
- [p5.js](https://p5js.org/) 包含给艺术家、设计师、教育者、初学者使用的完整的 canvas 绘制功能
- [Paper.js](http://paperjs.org/) 运行于 HTML5 Canvas 上的开源矢量图形脚本框架
- [Phaser](https://phaser.io/) 用于基于 Canvas 和 WebGL 的浏览器尤其的快速、自由、有趣的开源框架
- [Processing.js](https://processingjs.org/) 用于处理可视化语言
- [Pts.js](https://ptsjs.org/) 在 canvas 和 SVG 中进行创意性代码写作和可视化的库
- [Rekapi](https://github.com/jeremyckahn/rekapi) 关键帧动画库
- [Scrawl-canvas](https://scrawl.rikweb.org.uk/) 用来创建和编辑 2D 图形的开源库
- [ZIM](https://zimjs.com/) 框架为 canvas 上的代码创意性提供方便性、组件和可控性，包括可用性和数百个色彩缤纷的教程




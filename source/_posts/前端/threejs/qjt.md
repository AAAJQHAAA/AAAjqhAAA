---
title: 全景图展示技术选型
date: 2024-01-15 10:17:46
category: 前端
tags:
  - 全景图展示
---
# 全景图展示技术选型
- 全景图片展示，需要支持热点
- 目标做一个全景图编辑界面，修改配置，绘制热点等功能，绘制完成之后，可以生成链接访问
- 全景图格式
  - 等距柱状投影`either equirectangular`：最简单，只需单张图片，最好限制最大图像大小宽度为4096px
  - 立方体贴图`cube map`：需要六张图像，但支持更高分辨率的全景图，由于绝大多数设备支持最大4096像素的立方体面
  - 多分辨率格式`multiresolution formats`：此格式是基于立方体贴图的格式，但每个立方体面都是平铺图像金字塔，而不是单个图像。（缺点是需要额外的工作将图像转换为该图像以及必须托管的大量文件）

## three.js
- github：`https://github.com/mrdoob/three.js.git`
- 教程网站：`https://threejs.org/`
- CND引入、npm引入

## Panolens【不推荐】
- github：`https://github.com/pchen66/panolens.js.git`
- 教程网站：`https://pchen66.github.io/Panolens/`
- 基于threeJS的库，已经不更新了
- API参数没有说明（难用），效果不好
- CDN引入
- demo运行：需要`three.min.js`、`panolens.min.js`、`view.jpg`(全景图)
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>测试Panolens</title>
    <style>
        html,body{
            padding: 0;
            margin: 0;
            width:100%;
            height:100%;
            overflow: hidden;
        }
        #container {
            width:100%;
            height:100%;
        }
    </style>
</head>
<body>
<div id="container"></div>
<script src="../js/three.min.js"></script>
<script src="../js/panolens.min.js"></script>、
<script>
    const container = document.querySelector('#container');
    const viewer = new PANOLENS.Viewer({ container: container, controlBar: false });
    const panorama = new PANOLENS.ImagePanorama('../view.jpg');
    panorama.addEventListener('enter-fade-start', function(position){
      viewer.setControlCenter(position);
    }.bind( this, new THREE.Vector3( -5000.00, 167.06, 3449.90 )));
    viewer.add(panorama);
</script>
</body>
</html>
```

## Pannellum
- github：`https://github.com/mpetroff/pannellum.git`
- 教程网站：`https://pannellum.org/`
- 基于webgl，有参数说明
- CDN引入
- demo运行：需要`pannellum.css`、`pannellum.js`、`view.jpg`(全景图)
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>测试Pannellum</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/pannellum@2.5.6/build/pannellum.css"/>
    <script type="text/javascript" src="https://cdn.jsdelivr.net/npm/pannellum@2.5.6/build/pannellum.js"></script>
    <style>
        html,body{
            padding: 0;
            margin: 0;
            width:100%;
            height:100%;
            overflow: hidden;
        }
        #container {
            width:100%;
            height:100%;
        }
    </style>
</head>
<body>
<div id="container"></div>
<script>
pannellum.viewer('container', {
    "type": "equirectangular",// 全景类型：cubemap、multires、equirectangular
    "panorama": "../view.jpg",
    "autoLoad": true, // 自动加载
    "autoRotate": -2,  // 自动旋转
    //"preview": "/images/xxx.jpg", // 预览的图片
    "title": "我的标题", // 标题
    "author": "作者JQH", // 作者
    "compass": true,  // 指南针
    "northOffset": 247.5, // 北方偏移量
    "pitch": 2.3,  // 起始俯仰位置
    "yaw": 150, // 起始偏航位置
    "hfov": 130 // 起始水平视野
});
</script>
</body>
</html>
```

## Marzipano
- github：`https://github.com/google/marzipano.git`
- marzipano-tool：`https://github.com/foyez/marzipano-tool.git`
- 教程网站：`https://www.marzipano.net/`
  - API文档：`https://www.marzipano.net/reference/`
- 基于webgl，有参数说明
- CND引入、npm引入
- demo运行：需要`marzipano.js`、`view.jpg`(全景图)
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>测试Marzipano</title>
  <meta name="viewport" content="target-densitydpi=device-dpi, width=device-width, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, user-scalable=no, minimal-ui" />
  <style> @-ms-viewport { width: device-width; } </style>
  <script type="text/javascript" src="../js/marzipano.js"></script>
  <style>
        * {
          -webkit-box-sizing: border-box;
          -moz-box-sizing: border-box;
          box-sizing: border-box;
          -moz-user-select: none;
          -webkit-user-select: none;
          -ms-user-select: none;
          user-select: none;
          -webkit-user-drag: none;
          -webkit-touch-callout: none;
          -ms-content-zooming: none;
        }

        html, body {
          width: 100%;
          height: 100%;
          padding: 0;
          margin: 0;
          overflow: hidden;
        }

        #container {
          position: absolute;
          top: 0;
          left: 0;
          width: 100%;
          height: 100%;
        }
    </style>
</head>
<body>
<div id="container"></div>
<script>
    // 创建viewer.
    var viewer = new Marzipano.Viewer(document.getElementById('container'));
    // 创建source.
    var source = Marzipano.ImageUrlSource.fromString(
      "../view.jpg"
    );
    // 创建geometry.
    var geometry = new Marzipano.EquirectGeometry([{ width: 4000 }]);
    // 创建view.
    var limiter = Marzipano.RectilinearView.limit.traditional(1024, 100*Math.PI/180);
    var view = new Marzipano.RectilinearView({ yaw: Math.PI }, limiter);
    // 创建scene.
    var scene = viewer.createScene({
      source: source,
      geometry: geometry,
      view: view,
      pinFirstLevel: true
    });
    // 展示scene.
    scene.switchTo();
</script>
</body>
</html>
```

## view360【不推荐】
- 教程网站：`https://naver.github.io/egjs-view360/`
- 基于threejs
- CND引入、npm引入
- v4版本在测试阶段，有API文档【v3版本有点模糊】
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>测试view360</title>
    <link rel="stylesheet" href="https://naver.github.io/egjs-view360/release/latest/css/view360.min.css">
    <script src="../js/view360.pkgd.min.js"></script>
    <style>
          html,body{
              padding: 0;
              margin: 0;
              width:100%;
              height:100%;
              overflow: hidden;
          }
          #container {
              width:100%;
              height:100%;
          }
    </style>
</head>
<body>
  <div class="view360-container is-16by9" id="container">
    <canvas class="view360-canvas"/>
  </div>
  <script>
    const PanoViewer = eg.view360.PanoViewer;
    const panoViewer = new PanoViewer(
      document.getElementById("container"),
      {
        image: "../view.jpg"
      }
    );
  </script>
</body>
</html>
```

## Photo Sphere Viewer
- github：`https://github.com/mistic100/Photo-Sphere-Viewer.git`
- 教程网站：`https://photo-sphere-viewer.js.org/guide/`
- 基于threejs
- CND引入、npm引入
- 有API文档
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>测试Photo Sphere Viewer</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@photo-sphere-viewer/core/index.min.css" />
    <style>
          html,body{
              padding: 0;
              margin: 0;
              width:100%;
              height:100%;
              overflow: hidden;
          }
          #container {
              width:100%;
              height:100%;
          }
      </style>
</head>
<body>
<div id="container" style="width: 100vw; height: 100vh;"></div>

<script type="importmap">
    {
        "imports": {
            "three": "https://cdn.jsdelivr.net/npm/three/build/three.module.js",
            "@photo-sphere-viewer/core": "https://cdn.jsdelivr.net/npm/@photo-sphere-viewer/core/index.module.js"
        }
    }
</script>

<script type="module">
    import { Viewer } from '@photo-sphere-viewer/core';

    const viewer = new Viewer({
        container: document.querySelector('#container'),
        panorama: '../view.jpg',
    });
</script>
</body>
</html>
```

## 全景图概念
- 摄像机的视角和观察方向
  - Pitch（俯仰角）：俯仰角描述了摄像机的垂直旋转角度，即相对于地平线的角度。当俯仰角为0时，摄像机视线水平朝向前方；当俯仰角为正值时，摄像机视线向上偏离地平线；当俯仰角为负值时，摄像机向下偏离地平线。
  - Yaw（偏航角）：偏航角描述了摄像机的水平旋转角度，即相对于参考方向（通常为正北方向）的角度。当偏航角为0时，摄像机视线指向参考方向；当偏航角为正值时，摄像机顺时针旋转；当偏航角为负值时，摄像机逆时针旋转。
  - HFOV（水平视场角）：水平视场角描述了摄像机在水平方向上能够捕捉到的视野范围。它表示了从摄像机视线的左侧到右侧所能看到的角度范围。
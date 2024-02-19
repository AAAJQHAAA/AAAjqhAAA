---
title: Pannellum实现全景图编辑&预览
date: 2024-01-17 09:03:03
category: 前端
tags:
  - 全景图展示
  - Pannellum
  - webgl
---
# Pannellum实现全景图编辑&预览
- 全景图片展示，需要支持热点
- 目标做一个全景图编辑界面，修改配置，绘制热点等功能，绘制完成之后，可以生成链接访问
## 介绍
- github：`https://github.com/mpetroff/pannellum.git`
- 教程网站：`https://pannellum.org/`
- Pannellum是一个轻量级、免费和开源的Web全景查看器。它使用HTML5、CSS3、JavaScript和WebGL构建，无需插件。
## 依赖下载
```html
<!-- 最新编译和缩小的独立查看器 -->
https://cdn.pannellum.org/2.5/pannellum.htm
<!-- 最新编译和缩小的JavaScript -->
<script src="https://cdn.jsdelivr.net/npm/pannellum@2.5.6/build/pannellum.js"></script>
<!-- 最新编译和缩小的CSS -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/pannellum@2.5.6/build/pannellum.css">
```
- 全景图格式为`equirectangular`等距柱状投影，可以直接使用`pannellum.htm`访问打开
  - https://cdn.pannellum.org/2.5/pannellum.htm#panorama=https://pannellum.org/images/alma.jpg
- `pannellum.htm`访问打开也可以把配置写在json文件里面（`config`指定文件地址）
  - https://cdn.pannellum.org/2.5/pannellum.htm#config=https://pannellum.org/configs/compass.json
  - JSON配置选项
```json
{
  "type":"【String】【选填】指定全景类型，cubemap、multires、equirectangular（默认）",
  "title":"【String】【选填】全景图的标题",
  "author":"【String】【选填】全景图的作者",
  "authorURL":"【String】【选填】全景图的作者URL，点击名字跳转",
  "strings":"【词典】【选填】不知道啥用",
  "autoLoad":"【Boolean】【选填】自动加载，默认false",
  "autoRotate":"【int】【选填】加载成功后自动旋转速度，正为逆时针、负为顺时针",
  "autoRotateInactivityDelay":"【int】【选填】用户活动停止后自动旋转速度，正为逆时针、负为顺时针【前提：autoRotate】",
  "autoRotateStopDelay":"【int】【选填】设置延迟多少毫秒后停止自动旋转【前提：autoRotate】",
  "fallback":"【String】【选填】用户设备或者链接不工作时，替换方案的URL",
  "orientationOnByDefault":"【Boolean】【选填】默认开启设备方向控制",
  "showZoomCtrl":"【Boolean】【选填】显示缩放控件",
  "keyboardZoom":"【Boolean】【选填】使用键盘缩放",
  "mouseZoom":"【Boolean、String】【选填】使用鼠标滚轮进行缩放false、true、fullscreenonly（只有全屏时支持）",
  "draggable":"【Boolean】【选填】鼠标和触摸拖动，默认true",
  "friction":"【number,0-1之间】【选填】惯性摩擦力，默认0.15",
  "disableKeyboardCtrl":"【Boolean】【选填】禁用键盘控件",
  "showFullscreenCtrl":"【Boolean】【选填】显示全屏控件",
  "showControls":"【Boolean】【选填】显示控件",
  "touchPanSpeedCoeffFactor":"【int】【选填】调整触摸输入的平移速度,默认1",
  "yaw":"【int】【选填】全景图的起始偏航位置（以度为单位）,默认0",
  "pitch":"【int】【选填】全景图的起始俯仰位置（以度为单位）,默认0",
  "hfov":"【int】【选填】全景图的起始水平视野（以度为单位）,默认100",
  "minYaw":"【int】【选填】全景图的最小偏航（以度为单位）,默认-180",
  "maxYaw":"【int】【选填】全景图的最大偏航（以度为单位）,默认180",
  "minPitch":"【int】【选填】全景图的最小间距（以度为单位）,默认-90",
  "maxPitch":"【int】【选填】全景图的最大间距（以度为单位）,默认90",
  "minHfov":"【int】【选填】全景图的最小水平视野（以度为单位）,默认50",
  "maxHfov":"【int】【选填】全景图的最大水平视野（以度为单位）,默认120",
  "multiResMinHfov":"【Boolean】【选填】自动计算的最小水平视野",
  "compass":"【Boolean】【选填】显示指南针",
  "northOffset":"【int】【选填】全景图中心与北的偏移量【前提：compass】",
  "preview":"【String】【选填】加载全景图之前要显示的预览图像的URL",
  "previewTitle":"【String】【选填】显示加载按钮时要显示的标题",
  "previewAuthor":"【String】【选填】显示加载按钮时要显示的作者",
  "horizonPitch":"【int】【选填】图像地平线的俯仰，以度为单位（用于校正非分级全景图）",
  "horizonRoll":"【int】【选填】图像地平线的俯仰，以度为单位（用于校正非分级全景图）",
  "animationTimingFunction":"【Function】【选填】指定了一个计时函数，用于对运动进行动画处理",
  "escapeHTML":"【Boolean】【选填】如果为true，则HTML会从配置字符串中转义，以帮助缓解可能的情况DOM XSS攻击",
  "crossOrigin":"【String】【选填】指定了使用的CORS请求的类型",
  "hotSpots":"【Object】【选填】这指定了一个热点字典，可以链接到其他场景，信息或外部链接。每个数组元素都具有以下属性",
  "- pitch":"【int】【选填】指定热点位置的俯仰部分（以度为单位）",
  "- yaw":"【int】【选填】指定热点位置的偏航部分（以度为单位）",
  "- type":"【String】【选填】指定热点的类型。可以用于场景链接scene、信息热点info",
  "- text":"【String】【选填】指定了当用户将鼠标悬停在热点上时显示的文本",
  "- attributes":"【String】【选填】指定URL的链接属性,target当前页面、_blank新窗口",
  "- sceneId":"【String】【选填】指定要链接到的热点场景scene的ID，info不能用",
  "- targetPitch":"【int】【选填】指定目标场景的俯仰（以度为单位）",
  "- targetYaw":"【int】【选填】指定目标场景的偏航（以度为单位）",
  "- targetHfov":"【int】【选填】指定目标场景的 HFOV（以度为单位）",
  "- id":"【int】【选填】指定热点ID，用于API的函数",
  "- cssClass":"【String】【选填】热点的CSS类",
  "- createTooltipFunc":"【Function】【选填】创建热点DOM的函数",
  "- createTooltipArgs":"【Object】【选填】创建热点DOM的函数参数",
  "- clickHandlerFunc":"【Function】【选填】热点事件处理的函数",
  "- clickHandlerArgs":"【Object】【选填】热点事件处理的函数参数",
  "- scale":"【Boolean】【选填】对热点进行缩放以匹配视场的变化",
  "hotSpotDebug":"【Boolean】【选填】单击鼠标按钮，鼠标指针的俯仰和偏航将记录到控制台",
  "sceneFadeDuration":"【int】【选填】",
  "capturedKeyNumbers":"【阵列】【选填】",
  "backgroundColor":"【int，int，int】【选填】",
  "avoidShowingBackground":"【Boolean】【选填】防止显示部分全景图的超出范围的区域",
  "panorama":"【类型：equirectangular】【String】【选填】设置等距柱状投影全景图像的URL",
  "haov":"【类型：equirectangular】【int】【选填】设置全景图的水平视角（以度为单位）",
  "vaov":"【类型：equirectangular】【int】【选填】设置全景图的垂直视角（以度为单位）",
  "vOffset":"【类型：equirectangular】【int】【选填】设置等距柱状柱状投影图像中心的垂直偏移量地平线（以度为单位）",
  "cubeMap":"【类型：cubeMap】【Array】【选填】六个立方体面的URL数组",
  "basePath":"【类型：multires】【String】【选填】加载图像的基本路径",
  "path":"【类型：multires】【String】【选填】这是多分辨率切片位置的格式字符串",
  "fallbackPath":"【类型：multires】【String】【选填】这是CSS 3D的回退磁贴位置的格式字符串",
  "extension":"【类型：multires】【String】【选填】指定切片的文件扩展名",
  "tileResolution":"【类型：multires】【int】【选填】这指定了每个图像图块的大小（以像素为单位）",
  "maxLevel":"【类型：multires】【int】【选填】这指定了最大缩放级别",
  "cubeResolution":"【类型：multires】【int】【选填】这指定了面向图像的全分辨率立方体的大小（以像素为单位）"
}
```
## API事件
- `load`：在全景图完成加载时触发。
- `scenechange`：在场景更改时触发。当load新场景完成加载。将`场景ID字符串`传递给处理程序。
- `fullscreenchange`：在浏览器全屏状态更改时触发。将状态布尔值传递给处理程序。
- `zoomchange`：在场景hfov更新时触发。将新的hfov值传递给处理程序。
- `scenechangefadedone`：如果指定了场景过渡淡入淡出间隔，则在淡入淡出在切换场景后完成。
- `animatefinished`：当任何移动/动画完成时触发，即当渲染器停止时渲染新帧。将pitch、yaw、hfov传递给处理程序。
- `error`：发生错误时触发。错误消息字符串将传递给事件侦听器。
- `errorcleared`：清除错误时触发。
- `mousedown`：按下鼠标按钮时触发。MouseEvent传递给处理程序。
- `mouseup`：释放鼠标按钮时触发。MouseEvent传递给处理程序。
- `touchstart`：在触摸开始时触发。TouchEvent传递给处理程序。
- `touchend`：触摸结束时触发。TouchEvent传递给处理程序。


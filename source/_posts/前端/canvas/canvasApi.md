---
title: canvasApi
date: 2024-02-01 11:07:24
category: 前端
tags:
  - canvas 
  - canvas API
---
# canvas API
## HTMLCanvasElement

```js
/** 提供用于操作<canvas>元素的布局和表示的属性和方法。HTMLCanvasElement接口也继承了HTMLElement接口的属性和方法。 */
class HTMLCanvasElement extends HTMLElement {
    height: number;// 画布元素的高度。
    width: number;// 画布元素的宽度。
    /**
     * 返回一个对象，该对象提供用于在文档中的画布元素上绘制和操作图像和图形的方法和属性。上下文对象包括有关颜色、线宽、字体和其他可以在画布上绘制的图形参数的信息。
     * @param contextId 要创建的画布类型的标识符(ID)。IE 9和IE 10只支持2d上下文;IE11支持使用canvas.getContext("experimental-webgl")的3d或WebGL上下文;
     */
    getContext(contextId: "2d", options?: CanvasRenderingContext2DSettings): CanvasRenderingContext2D | null;
    getContext(contextId: "bitmaprenderer", options?: ImageBitmapRenderingContextSettings): ImageBitmapRenderingContext | null;
    getContext(contextId: "webgl", options?: WebGLContextAttributes): WebGLRenderingContext | null;
    getContext(contextId: "webgl2", options?: WebGLContextAttributes): WebGL2RenderingContext | null;
    getContext(contextId: string, options?: any): RenderingContext | null;
    // 画布中包含的图像
    toBlob(callback: BlobCallback, type?: string, quality?: any): void;
    // 将当前画布的内容作为图像返回，您可以将该图像用作另一个画布或HTML元素的源。
    toDataURL(type?: string, quality?: any): string;
    // 对画布内容的实时视频捕获
    captureStream(frameRate?: number): CanvasCaptureMediaStream;
    // 将控制权转移到主线程或工作线程上
    transferControlToOffscreen(): OffscreenCanvas;
}
```

## CanvasRenderingContext2D

```js
class CanvasRenderingContext2D {
  canvas: HTMLCanvasElement;// canvas只读引用
  width: number;// 画布元素的高度。
  height: number;// 画布元素的宽度。
  // 对于没有直接固定到特定画布的上下文
  commit(): void;
  
  // 状态
  // 保存 当前状态 到堆栈上
  save(): void;
  // 弹出 图形状态堆栈中的 顶部条目
  restore(): void;

  // 转换
  currentTransform: SVGMatrix;// 当前的变换
  // （坐标系）缩放 水平或垂直
  scale(x: number, y: number): void;
  // （坐标系）旋转（弧度 = 角度 * Math.PI / 180）
  rotate(angle: number): void;
  // （坐标系）移动
  translate(x: number, y: number): void;
  // （坐标系）矩阵变换：缩放(a,d)、旋转(b,c)、平移(e,f)（相对变换矩阵，在当前基础上变换）
  transform(a: number, b: number, c: number, d: number, e: number, f: number): void;
  // （坐标系）矩阵变换：缩放(a,d)、旋转(b,c)、平移(e,f)（绝对变换矩阵，取消当前变换设置成新的变换）
  setTransform(a: number, b: number, c: number, d: number, e: number, f: number): void;
  // 重新设置
  resetTransform(): void;

  // 合成
  globalAlpha: number;// 设置图形和图片透明度(0.0-1.0)
  globalCompositeOperation: string;// 设置绘制新形状时应用的合成操作的类型

  // 图像的平滑
  imageSmoothingEnabled: boolean;// 设置图片是否平滑
  imageSmoothingQuality: 'low' | 'medium' | 'high';// 设置图像平滑度的属性

  // 过滤器
  filter: string;// 提供模糊、灰度等过滤效果的属性

  // 颜色和款式
  strokeStyle: string | CanvasGradient | CanvasPattern;// 设置上下文颜色(描边)
  fillStyle: string | CanvasGradient | CanvasPattern;// 设置上下文颜色(填充)
  // 两点之间构建线性渐变，返回值赋值给 fillStyle 或者 strokeStyle
  createLinearGradient(x0: number, y0: number, x1: number, y1: number): CanvasGradient;
  // 两圆之间构建放射性渐变
  createRadialGradient(x0: number, y0: number, r0: number, x1: number, y1: number, r1: number): CanvasGradient;
  // 图像背景（填充），返回值赋值给 fillStyle
  createPattern(image: CanvasImageSource, repetition: ?string): CanvasPattern;
  // 指定点周围创建渐变（开始弧度，坐标x，坐标y）
  createConicGradient(startAngle, x, y): CanvasGradient;


  // 阴影
  shadowOffsetX: number;// 阴影水平偏移距离
  shadowOffsetY: number;// 阴影垂直偏移距离
  shadowBlur: number;// 模糊效果程度
  shadowColor: string;// 阴影颜色

  // 矩形
  // 清除区域(x坐标，y坐标，宽，高)【透明】
  clearRect(x: number, y: number, w: number, h: number): void;
  // 绘制填充矩形(x坐标，y坐标，宽，高)
  fillRect(x: number, y: number, w: number, h: number): void;
  // 绘制描边矩形(x坐标，y坐标，宽，高)
  strokeRect(x: number, y: number, w: number, h: number): void;

  // 路径API
  // 清空之前子路径，开始新的路径绘制
  beginPath(): void;
  // （填充）根据当前的填充样式，填充当前或已存在的路径。采取非零环绕或者奇偶环绕规则
  fill(fillRule?: CanvasFillRule): void;
  fill(path: Path2D, fillRule?: CanvasFillRule): void;
  // （描边）使用非零环绕规则，根据当前的画线样式，绘制当前或已经存在的路径
  stroke(): void;
  stroke(path: Path2D): void;
  // 用来给当前路径或特定路径绘制焦点的方法，如果给定的元素获取了焦点。
  drawFocusIfNeeded(element: Element): void;
  drawFocusIfNeeded(path: Path2D, element: Element): void;
  // 将当前或给定的路径滚动到窗口的方法
  scrollPathIntoView(): void;
  scrollPathIntoView(path: Path2D): void;
  // 路径裁剪（裁剪出有效区域，浏览器会将后面所有的绘图操作都限制在本区域内执行）
  clip(fillRule?: CanvasFillRule): void;
  clip(path: Path2D, fillRule?: CanvasFillRule): void;
  resetClip(): void;
  // 用于判断在当前路径中是否包含检测点的方法
  isPointInPath(x: number, y: number, fillRule?: CanvasFillRule): boolean;
  isPointInPath(path: Path2D, x: number, y: number, fillRule?: CanvasFillRule): boolean;
  // 用于检测某点是否在路径的描边线上的方法
  isPointInStroke(x: number, y: number): boolean;
  isPointInStroke(path: Path2D, x: number, y: number): boolean;

  // 文本(参见CanvasDrawingStyles接口)
  // （填充）在指定的坐标上绘制文本
  fillText(text: string, x: number, y: number, maxWidth?: number): void;
  // （描边）在给定的 (x, y) 位置绘制文本
  strokeText(text: string, x: number, y: number, maxWidth?: number): void;
  // 被测量文本信息
  measureText(text: string): TextMetrics;

  // 画图片
  // 绘图+移动
  drawImage(image: CanvasImageSource, dx: number, dy: number): void;
  // 绘图+移动+缩放（dx,dy,dw,dh视图）
  drawImage(image: CanvasImageSource, dx: number, dy: number, dw: number, dh: number): void;
  // 绘图+裁剪+移动+缩放（sx,sy,sw,sh相机视口、dx,dy,dw,dh视图）
  drawImage(image: CanvasImageSource, sx: number, sy: number, sw: number, sh: number, dx: number, dy: number, dw: number, dh: number): void;

  // 点击区域
  addHitRegion(options?: HitRegionOptions): void;
  removeHitRegion(id: string): void;
  clearHitRegions(): void;

  // 像素操作
  // 创建一个新的、空白的、指定大小的 ImageData 对象。所有的像素在新对象中都是透明的
  createImageData(sw: number, sh: number): ImageData;
  createImageData(imagedata: ImageData): ImageData;
  // 获取描述 canvas 区域隐含的像素数据
  getImageData(sx: number, sy: number, sw: number, sh: number): ImageData;
  putImageData(imagedata: ImageData, dx: number, dy: number): void;
  putImageData(imagedata: ImageData, dx: number, dy: number, dirtyX: number, dirtyY: number, dirtyWidth: number, dirtyHeight: number): void;

  // 画布绘画风格
  lineWidth: number;// 设置线段厚度（描边）
  lineCap: string;// 线段端点样式（描边）
  lineJoin: string;// 线段拐角样式（描边）
  miterLimit: number;// 拐角最大厚度（描边）

  // 虚线
  // 描边设置为虚线（[实、虚、实、虚....]）
  setLineDash(segments: Array<number>): void;
  // 获取当前线段样式的方法
  getLineDash(): Array<number>;
  lineDashOffset: number;// 设置虚线偏移量（前面）

  // 文本
  font: string;// 当前字体样式
  textAlign: string;// 文本的对齐方式
  textBaseline: string;// 当前文本基线
  direction: string;// 当前文本方向

  // 路径方法
  // 共享路径API方法
  // 将笔点连到当前子路径起始点的方法
  closePath(): void;
  // 将一个新的子路径的起始点移动到 (x，y) 坐标
  moveTo(x: number, y: number): void;
  // 使用直线连接子路径的终点到 x，y 坐标
  lineTo(x: number, y: number): void;
  // 新增二次贝塞尔曲线路径（控制点x,控制点y,端点x,端点y）
  quadraticCurveTo(cpx: number, cpy: number, x: number, y: number): void;
  // 绘制三次贝赛尔曲线路径（控制点x,控制点y,控制点x2,控制点y2,端点x,端点y）
  bezierCurveTo(cp1x: number, cp1y: number, cp2x: number, cp2y: number, x: number, y: number): void;
  // 根据控制点和半径绘制圆弧路径，使用当前的描点 (前一个 moveTo 或 lineTo 等函数的止点)（控制点x,控制点y,控制点x2,控制点y2,弧度）
  arcTo(x1: number, y1: number, x2: number, y2: number, radius: number): void;
  arcTo(x1: number, y1: number, x2: number, y2: number, radiusX: number, radiusY: number, rotation: number): void;
  // 创建矩形路径的方法，矩形的起点位置是 (x, y)，尺寸为 width 和 height。矩形的 4 个点通过直线连接，子路径做为闭合的标记，所以你可以填充或者描边矩形
  rect(x: number, y: number, w: number, h: number): void;
  // 绘制圆弧路径(x,y,半径,开始弧度,结束弧度,是否逆时针方向)
  arc(x: number, y: number, radius: number, startAngle: number, endAngle: number, anticlockwise?: boolean): void;
  // 添加椭圆路径的方法
  ellipse(x: number, y: number, radiusX: number, radiusY: number, rotation: number, startAngle: number, endAngle: number, anticlockwise?: boolean): void;
}
```

## Path2D CanvasPath

```js
interface CanvasPath {
    arc(x: number, y: number, radius: number, startAngle: number, endAngle: number, anticlockwise?: boolean): void;
    arcTo(x1: number, y1: number, x2: number, y2: number, radius: number): void;
    bezierCurveTo(cp1x: number, cp1y: number, cp2x: number, cp2y: number, x: number, y: number): void;
    closePath(): void;
    ellipse(x: number, y: number, radiusX: number, radiusY: number, rotation: number, startAngle: number, endAngle: number, anticlockwise?: boolean): void;
    lineTo(x: number, y: number): void;
    moveTo(x: number, y: number): void;
    quadraticCurveTo(cpx: number, cpy: number, x: number, y: number): void;
    rect(x: number, y: number, w: number, h: number): void;
}
```

```js
interface Path2D extends CanvasPath {
    /**
     * Adds to the path the path given by the argument.
     */
    addPath(path: Path2D, transform?: DOMMatrix2DInit): void;
}
```

## ImageBitmapRenderingContext

## WebGLRenderingContext

## WebGL2RenderingContext

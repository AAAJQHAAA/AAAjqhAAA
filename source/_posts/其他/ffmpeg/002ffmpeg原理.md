# ffmpeg原理
- https://ffmpeg.xianwaizhiyin.net/base-knowledge/raw-rgb.html
## 音视频基础知识
- 1、RGB（Red红、Green绿、Blue蓝）色彩空间（图像采集&显示）
  - 使用RGB的文件格式：BMP
  - 查看文件的二进制软件：[hxd软件](https://mh-nexus.de/en/hxd/)
  - 图片和BMP互转：`ffmpeg -i test.jpg test.bmp` & `ffmpeg -i test.bmp test.jpg`
- 2、YUV（Y亮度，U色调，V色饱和度）色彩空间（编码&存储）
  - 存储之前：RGB--转换为-->YUV
  - 显示之前：YUV--转换为-->RGB
- 3、编码压缩（encode）
  - YUV的数据还是太大了，需要进一步压缩
  - 压缩标准： JPEG、H.262、H.263、H.264、VP9、AVS
- 4、封装格式
  - 把多个流数据合并到一个文件里面，这就是封装格式，这就是 mux（复用）
  - 封装格式就是一个数据结构，哪个数据在哪个地方，都是固定好的
  - 
## ffmpeg基础
## 编译链接基础知识
## msys2从入门到精通
## ffmpeg调试环境搭建
## ffmpeg编译教程-高级篇
## 如何引入ffmpeg的API库
## ffmpeg数据结构详解
## ffmpeg滤镜API教程
## ffmpeg高级API教程
## Libavutil库介绍
## ffmpeg编译过程分析
## ffplay.c源码分析
## ffmpeg.c源码分析

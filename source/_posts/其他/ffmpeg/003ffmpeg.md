# 音视频基础与ffmpeg
## 问题
- ffmpeg推流命令：`ffmpeg -i ~/xxx/xxx.mp4 -f flv rtmp://localhost/live/test`
- ffplay拉流命令：`ffplay rtmp://localhost/live/test`
- 卡顿：音频和视频不同步（加参数-re音视频同步播放）
- 不清晰问题：视频质量重编码码（加参数-c:v copy视频采用原来的编码）
## 基础知识
### linux基础知识
- 基础命令：`ls`、`cd`、`pwd`、`mkdir`、`cp`、`rm`、`mv`、`sudo`、`pkg-config`
- 安装工具：`apt`、`brew`、`yum`
- vim基础操作：`:w`、`:q`、`:wq`、`i`、`esc`、`h`、`j`、`k`、`l`
- linux中的环境变量中的：`PATH`、`PKG_CONFIG_PATH`
    - 修改设置环境变量：`～/.bashrc`、`～/bash_profile`
    - 生效环境变量：`source`
    - 路径分隔符：`;`、`:`
    - `env`命令查看所有环境变量
- `pkg-config --libs --cflags libavutil`查看`libavutil`的头文件和库文件地址
    - 引入ffmpeg头文件和库文件的需要知道位置
    - `PKG_CONFIG_PATH`中需配置ffmpeg的`/usr/local/ffmpeg/lib/pkgconfig`的路径
    - `pkg-config`会在`PKG_CONFIG_PATH`路径下找`libavutil`
- 问题PKG_CONFIG_PATH与LD_LIBRARY_PATH区别
### C基础回顾
### Swift语言
### 各平台ffmpeg编译
## 音频
### 音频基础知识
### 音频编码原理
### 音频数据的采集
### AAC/OPUS编码
## 视频
### RGB/YUV
### H264编码原理
### 视频数据采集
### H264/H265编码
### H264参数调优
## 流媒体服务器
### nginx
### SRS
### CDN网络
### RTMP传输














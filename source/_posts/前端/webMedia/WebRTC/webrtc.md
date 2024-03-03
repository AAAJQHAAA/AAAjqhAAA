---
title: webrtc
date: 2024-03-01 15:46:45
category: 前端
tags:
  - webrtc
---


# WebRTC协议
- 会话描述协议(SDP)[SDP文档](https://datatracker.ietf.org/doc/html/rfc4566#section-1)
    - 会话发起协议(SIP)参数[SDP参数](https://www.iana.org/assignments/sip-parameters/sip-parameters.xhtml)
        - 会话名称和意图描述
        - v = （协议版本）
        - o = （所有者/创建者和会话标识符）
        - s = （会话名称）
        - i = * （会话信息）
        - u = * （URI 描述）
        - e = * （Email 地址）
        - p = * （电话号码）
        - c = * （连接信息 ― 如果包含在所有媒体中，则不需要该字段）
        - b = * （带宽信息）
        - 时间描述
        - t = （会话活动时间）
        - r = * （0或多次重复次数）
        - 媒体描述
        - m = （媒体名称和传输地址）
        - i = * （媒体标题）
        - c = * （连接信息 — 如果包含在会话层则该字段可选）
        - b = * （带宽信息）
        - k = * （加密密钥）
        - a = * （0 个或多个会话属性行）
- 流控制传输协议(SCTP)提供的服务与UDP和TCP类似
    - RTCSctpTransport,用于为对等连接上的任何和所有RTCDataChannel发送和接收数据。
- 数据包传输层安全性协议(DTLS)(Datagram Transport Layer Security)
    - RTCDtlsTransport,当RTCPeerConnection调用setLocalDescription()或创建对象setRemoteDescription()时创建
    - DTLS传输的数量以及它们的使用方式取决于RTCPeerConnection创建时的参数bundlePolicy
        - bundlePolicy="balanced"//音频、视频和数据通道分别创建一个RTCDtlsTransport
        - bundlePolicy="max-compat"//每个传输负责处理给定类型媒体（音频、视频或数据通道）的所有通信
        - bundlePolicy="max-bundle"//只创建一个RTCDtlsTransport 来承载的所有RTCPeerConnection数据
    - 因为传输是在协商过程的早期建立的，所以很可能直到它们创建之后才能知道远程对等方是否支持捆绑
        - 您有时会看到一开始创建了单独的传输，每个轨道一个，然后在知道可以捆绑后看到它们被捆绑在一起

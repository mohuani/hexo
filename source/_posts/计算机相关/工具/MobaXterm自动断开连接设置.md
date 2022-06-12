---
title: "MobaXterm自动断开连接设置"
date: 2020-08-14 12:36:38
draft: true
tags: [MobaXterm]
categories:
- [MobaXterm]
---

#### 场景：
使用MobaXterm工具通过SSH连接Linux服务器，如果一段时间没有操作，MobaXterm会把连接自动断开，这个设定很是不方便。通过更改下面的设置可以使SSH保持长连接，不会自动断开。

- 点击设置
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190415215615318.png)
- 把 SSH keepalive 勾选上
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190415215709198.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
---
title: "nginx中间件架构-性能优化"
date: 2018-06-17 21:22:30
draft: true
tags: [NGINX, Nginx中间件架构]
categories:
- [NGINX, Nginx中间件架构]
---

#### location匹配优先级
 ~ 区分大小写，~~*不区分大小写
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190210204325483.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
#### try_files
场景：用于文件缓存
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190210205237263.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
#### root和alias的区别
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190210205600926.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190210205627916.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
#### 传递用户的真实ip
在代理1处直接获取用户的ip，然后通过层层代理透传到后端
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190210210042226.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
#### 其他常见错误
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190210210327967.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
#### 文件句柄优化
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190210213216549.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
 针对用户进行修改
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190210213355529.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
 针对进程限制
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190210213510966.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
 #### cpu亲和绑定
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190211223904157.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
#### gzip兼容
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190211225553944.png)
#### 安全
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190211230042274.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190211230546667.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
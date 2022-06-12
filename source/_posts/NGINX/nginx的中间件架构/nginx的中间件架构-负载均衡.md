---
title: "nginx中间件架构-负载均衡"
date: 2018-06-17 21:22:30
draft: true
tags: [NGINX, Nginx中间件架构]
categories:
- [NGINX, Nginx中间件架构]
---

#### upstream
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190208163713131.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
#### 负载均衡调度状态
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190208163801717.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
#### 调度算法
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019020816472012.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
#### url_hash

```
Syntax:	hash key [consistent];
Default:	-;
Context:	upstream;
this directive appeared in version 1.7.2
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190208165904559.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
#### 代理缓存
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190208170329684.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
#### proxy_cache
- 完整
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190208170515936.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
- 部分

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019020817072612.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
#### 缓存过期周期
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190208170807576.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
#### 缓存的维度
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190208170844518.png)
#### 如何清除指定缓存
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190208171702548.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
#### 如何让指定页面不进行缓存
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190208171754843.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
#### 大文件分片请求
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190208172159437.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190208172303566.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190208172232265.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
#### rewrite规则
- 这里的规则可以用于写一个维护页面
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190209222155472.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
#### flag
beak不会建立新的请求，last还会建立一个新的请求过来。向某个域名进行请求，301请求一次后再次请求不会经过服务器，302请求一次后再次请求还需求经过服务器
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190209223509646.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
#### rewrite规则优先级
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190209225518691.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019020922562388.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)


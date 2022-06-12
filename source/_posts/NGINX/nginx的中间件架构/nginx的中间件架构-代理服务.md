---
title: "nginx中间件架构-代理服务"
date: 2018-06-17 21:22:30
draft: true
tags: [NGINX, Nginx中间件架构]
categories:
- [NGINX, Nginx中间件架构]
---

#### 代理区别
正向代理代理的对象是客户端
反向代理代理的对象是服务端
#### proxy_pass 
```
配置语法
Syntax:		proxy_pass URL;
Default:	-;
Context:	location, if in location, limit_except;
------
http://localhost:8080/uri/
https://192.168.1.1:8080/uri/

```
#### 缓冲区

```
Syntax:		proxy_buffering on | off;
Default:	proxy_buffering on;
Context:	http, server, location;
拓展： proxy_buffer_size, proxy_buffers, proxy_busy_buffers_size
```
#### 跳转重定向
```
Syntax:		proxy_redirect default; proxy_redirect off; proxy_redirect redirect replacement;
Default:	proxy_redirect ;
Context:	server, location;
```
#### 头信息
```
Syntax:		proxy_set_header field value;
Default:	proxy_set_header Host $proxy_host;
				proxy_set_header Connection close;
Context:	server, location;
扩展：proxy_hide_header, proxy_set_body
```
#### 超时
```
Syntax:		proxy_connect_timeout time;
Default:	proxy_connect_timeout 60s;
Context:	server, location;
扩展：proxy_read_timeout, proxy_send_timeout  
```
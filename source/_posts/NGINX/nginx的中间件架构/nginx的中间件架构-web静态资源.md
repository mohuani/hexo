---
title: "nginx中间件架构-web静态资源"
date: 2018-06-17 21:22:30
draft: true
tags: [NGINX, Nginx中间件架构]
categories:
- [NGINX, Nginx中间件架构]
---

#### http_x_forwarded_for

```
http_x_forwarded_for = Client IP, Proxy1 IP, Client2 IP, Client3 IP...
```
#### http_auth_basic_module配置

```
Syntax:		auth_basic string | off;
Default:	auth_basic off;
Context:	http, server, location, limit_except
 ------
Syntax:		auth_basic_user_file file;
Default:	—
Context:	http, server, location, limit_except
 ------
 location ~ ^/admin.html {
		root	/opt/app/code;
		auth_basic 	"Auth access test ! imput your password !";
		auth_basic_user_file	/etc/nginx/auth_conf;  // auth_conf文件存放已经保存的用户名和密码
		index	index.html index.htm;
}

http_auth_basic_module的局限性：
1、用户信息依赖文件
2、操作管理机械，效率低下

解决方案：
1、nginx结合Lua实现高效验证
2、nginx和LDAP打通，利用nginx-auth-ladp模块
```
#### Nginx作为静态资源web服务_配置语法
 -	文件读取
```
Syntax:		sendfile on | off;
Default: 	sendfile off;
Context:	http, server, location, if in location
引读：-with-file-aio 异步文件读取
```
- tcp_nopush
```
Syntax:		tcp_nopush on | off;
Default: 	tcp_nopush off;
Context:	http, server, location;
作用：sendfile开启的情况下，提高网络包的传输效率
```
- tcp_nodelay

```
Syntax:		tcp_nodelay on | off;
Default: 	tcp_nodelay off;
Context:	http, server, location;
作用：keepalive连接下，提高网络包的额传输实时性
```
- 压缩

```
压缩
Syntax:		gzip on | off;
Default: 	gzip off;
Context:	http, server, location, if in location;
作用：压缩传输
------
压缩比
Syntax:		gzip_comp_level level;
Default: 	gzip_comp_level 1;
Context:	http, server, location;
------
压缩版本
Syntax:		gzip_http_version 1.0 | 1.1;
Default: 	gzip_http_version 1.1;
Context:	http, server, location;
------
拓展nginx压缩模块
http_gzip_static_module - 预读gzip功能
http_gunzip_module - 引用支持gunzip的压缩格式
```
#### expires
添加 Cache-Control、Expires头

```
Syntax:		expires [modified] time;
					expires eposh | max | off;
Default: 	expires off;
Context:	http, server, location, if in location;
```
#### 跨域访问
Access-Control-Allow-Origin
```
Syntax:		add_header name value [always];
Default: 	-;
Context:	http, server, location, if in location;
```
#### 防盗链 http_refer

```
Syntax:		valid_referers none | block | server_names | string...;
Default: 	-;
Context:	http, server, location;
------
valid_referers none blocked 116.62.103.228;
if ($valid_referer) {
	return 403;
}
root /opt/app/code/images;
示例

```
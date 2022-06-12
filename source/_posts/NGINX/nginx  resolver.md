---
title: "nginx  resolver"
date: 2018-06-22 21:22:30
draft: true
tags: [NGINX]
categories:
- [NGINX]
---

**背景**：nginx 配置proxy_pass后，访问接口出现no resolver defined to resolve错误，或者接口直接502，404，需要配置 nginx resolver

```
server {
	server_name wfk.mohuani.com
	
	location / {
	resolver 114.114.114.114;
	proxy_pass https://wfk.mohuani.com/abc/$1/$2  ---404
	proxy_pass https://www.baidu.com/abc/$1/$2  ---502
	proxy_pass https://113.105.77.194/abc/$1/$2  ---404
	proxy_pass https://127.0.0.1/abc/$1/$2   ---404
}

```

##### 不配置 resolver的情况接口的状态
 - proxy_pass 代理的是域名（非自身server_name），且代理的地址中包含变量，接口502
 - proxy_pass 代理的是ip，且代理的地址中包含变量，接口404
 - proxy_pass 代理的域名如果和自身server_name相同接口会404，如果和自身的server_name不相同的话接口会502

 ps：个人猜测，当proxy_pass 设置为自身server_name 时，server_name会被转成127.0.0.1，导致接口报的是404，而不是502。

##### 具体什么情况下需要配置 resolver
- proxy_pass 代理的是**域名**或者**ip**,且代理的接口中包含 **变量**

##### resolver设置哪些DNS
resolver 设置公共的DNS或者公司内部的DNS都可以

```
resolver 114.114.114.114;
resolver 8.8.8.8;
```

 


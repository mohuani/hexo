---
title: "docker-compose lnmp"
date: 2018-08-22 20:36:39
draft: true
tags: [运维, docker]
categories:
- [运维, docker]
---

### 前置条件
ubuntu 安装好 docker 和 docker-compose


#### 编写启动文件

- docker-compose.yml
```yaml
version: '3'
services:
  nginx:
    container_name: v-nginx
    image: nginx
    restart: always
    ports:
    - 80:80
    - 443:443
    volumes:
    - ./nginx/conf.d:/tmp/nginx/conf.d
    
  mysql:
    container_name: v-mysql
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: root
    ports:
    - "3306:3306"
    restart: always
    volumes:
    - ./mysql:/tmp/lib/mysql
```

运行命令

```bash
docker-compose -f ./docker-compose.yml up -d 
```
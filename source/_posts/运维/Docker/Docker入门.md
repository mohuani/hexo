---
title: "Docker入门"
date: 2018-08-22 20:36:39
draft: true
tags: [运维, docker]
categories:
- [运维, docker]
---

##### Docker入门
课程地址：https://www.imooc.com/learn/867

##### 原理图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190621221824749.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190621221925307.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
##### 常见命令
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190622020403160.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190622020416908.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
##### Dockerfile介绍
最简单的创建命令
```dockerfile
FROM alpine:latest
MAINTAINER xbf
CMD echo "Hello Docker"
```
然后通过以下命令进行创建，查看，运行

```shell
docker build -t hello_docker .  // 将该目录下的所有dockerfile文件都进行build
docker images hello_docker  // 列出hello_docker的相关信息
docker run hello_docker  // 运行hello_docker应用容器
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/201906220222401.png)
再次创建一个镜像并运行
```dockerfile
FROM ubuntu
MAINTAINER mohuani
RUN sed -i 's/archive.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list
RUN apt-get update
RUN apt-get install -y nginx
COPY index.html /var/www/html
ENTRYPOINT ["usr/sbin/nginx", "-g", "daemon off;"]
EXPOSE 80
```

```shell
docker run -d -p 8081:80 hello_nginx
curl "http://localhost::8081"
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190622025227208.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019062202532895.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
##### 存储
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190622030745734.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190622030812364.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190622031053524.png)
##### 镜像仓库
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190622031641121.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190622031906671.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190622031945395.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)

```shell'
docker search whalesay
docker images  // 此处可以忽略
docker run docker/whalesay cowsay 你真的是傻比

docker tag docker/whalesay mohuani/whalesay  // 以docker/whalesay为模板创建一个名字为mohuani/whalesay的镜像
docker login
docker push mohuani/whalesay
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190622032800383.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190622033555763.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
登录网站   https://hub.docker.com/ 就可以查看自己刚刚上传的镜像信息
##### 多容器app docker-compose
docker-compose的官方安装教程  https://docs.docker.com/compose/

```shell
sudo curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
docker-compose --version
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190622040044533.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190622040730314.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190622040709707.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190622044043339.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190622044131488.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
Ghost相关的链接
Ghost中文网  http://www.ghostchina.com/
Ghost的docker镜像 https://hub.docker.com/_/ghost
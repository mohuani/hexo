---
title: "win10设置ubuntu系统root账户密码"
date: 2019-09-05 19:36:38
draft: true
tags: [ubuntu]
categories:
- [ubuntu]
---


### 一、安装ubuntu
ubuntu安装docker官方文档： [https://docs.docker.com/install/linux/docker-ce/ubuntu/](https://docs.docker.com/install/linux/docker-ce/ubuntu/)

### 二、修改root账户的密码
在win10商店下载ubuntu，安装docker后，每次使用docker命令都需要输入sudo，不使用sudo会报错

```powershell
Cannot connect to the Docker daemon at tcp://localhost:2375. Is the docker daemon running?
```
网上也有其他的解决方法，我试了都不行，干脆进到ubuntu后，直接切换成root用户，这样就不用每次输入sudo，下面操作如下

```powershell
sudo passwd root
[sudo] password for mohuani:
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully

su root
Password:
```
可以愉快的玩耍了...
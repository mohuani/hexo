---
title: "Phpstorm ubuntu创建图标"
date: 2019-09-03 19:36:38
draft: true
tags: [Phpstorm]
categories:
- [Phpstorm]
---

我们通过源码包的方式安装Jetbrain全家桶的软件时，桌面是没有快捷方式启动的，只能通过命令行的方式启动，很不方便。下面贴一下我的配置，通过快捷方式启动软件
ubuntu 2004

进入目录下面的目录，参考里面已有的文件创建一个xxx.desktop文件
```bash
/usr/share/applications
```
### PhpStorm

```bash
[Desktop Entry]
Type=Application
Name=Phpstorm
GenericName=Phpstorm2020
Comment=Phpstorm2020:The PHP IDE
Exec="/home/mohuani/develop/PhpStorm-201.7846.90/bin/phpstorm.sh"
Icon=/home/mohuani/develop/PhpStorm-201.7846.90/bin/phpstorm.svg
Terminal=phpstorm
Categories=Phpstorm
```
### DataGrip

```bash
[Desktop Entry]
Type=Application
Name=Datagrip
GenericName=DataGrip2020
Comment=DataGrip2020:The SQL IDE
Exec="/home/mohuani/develop/DataGrip-2020.1.5/bin/datagrip.sh"
Icon=/home/mohuani/develop/DataGrip-2020.1.5/bin/datagrip.svg
Terminal=datagrip
Categories=Datagrip
```
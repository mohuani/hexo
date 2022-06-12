
---
title: "gitignore文件不生效"
date: 2020-06-15 21:22:30
draft: true
tags: [Git]
categories:
- [Git]
---

### 背景
添加了gitignore文件，设置了要过滤的文件，但是提交后发现要过滤的文件还是被提交上去了。
### 原因
在git忽略目录中，新建的文件在git中会有缓存，如果某些文件已经被纳入了版本管理中，就算是在.gitignore中已经声明了忽略路径也是不起作用的，
### 解决方法
这时候我们就应该先把本地缓存删除，然后再进行git的提交，这样就不会出现忽略的文件了。

```bash
git rm -r --cached .
git add .
git commit -m "remove .idea files"
git push origin master
```
![20200405013811329](../images/20200405013811329.png)


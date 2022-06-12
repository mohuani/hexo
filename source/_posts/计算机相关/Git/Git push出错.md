
---
title: "Git push出错"
date: 2020-06-15 21:22:30
draft: true
tags: [Git]
categories:
- [Git]
---

### 背景
本地一个项目，写的差不多的时候，想起来使用git进行版本管理，于是在远程git仓库创建了一个仓库，本地与远程进行关联后，git push时出现错误 error: failed to push some refs to

### 原因
远程仓库创建的时候，生成了一部分文件（readme，gitignore），本地第一次往远程提交的时候，发现两边的内容不相同，push失败。

### 解决方法
远程仓库创建好后，本地项目还未使用 git inti 初始化，如果已经初始化，请直接删除 .git 文件，然后执行下面的步骤

```powershell
 git init
 git remote add origin git@gitee.com:mohuani/bubble.git
 git pull --rebase origin master
 git add .
 git commit -m "add xxx"
 git push origin master
```

一般而言，正常的推送流程应为：

```bash
1、在github上创建项目

2、使用git clone https://github.com/name/project.git克隆到本地

3、编辑项目

4、git add . （将变更提交至缓存区）

5、git commit -m “提交说明（注释）”

6、git push origin master 将本地变更推送至远程仓库master分支
```

---
参考文章： 

> https://blog.csdn.net/MBuger/article/details/70197532
---
title: Mac关闭浏览器手势
date: 2021-08-17 19:32:47
draft: true
tags: [计算机相关, Mac]
categories:
- [计算机相关, Mac]
---

Mac上面关闭 双指左滑右滑，导致浏览器前进后退的现象

### Chrome

```
defaults write com.google.Chrome AppleEnableSwipeNavigateWithScrolls -bool false
```

### Microsoft Edge

```
defaults write com.microsoft.edgemac AppleEnableSwipeNavigateWithScrolls -bool false
```









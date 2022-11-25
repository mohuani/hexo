---
title: "Maven提高编译速度"
date: 2022-11-25 21:22:30
draft: true
tags: [Java, maven]
categories:
- [Java, maven]
---

从PHP转到Java技术栈后，每次项目跑CI，Maven都要编译好久，浪费时间，就想着有没有什么方法可以提交编译速度，让开发体验更友好一点。下面是找到的一个文档资料。

核心思想就是利用CPU多核进行并发构建

- https://cwiki.apache.org/confluence/display/MAVEN/Parallel+builds+in+Maven+3
- http://www.codebaoku.com/it-java/it-java-223593.html

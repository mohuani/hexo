---
title: "composer安装依赖报错"
date: 2021-07-22 19:22:30
draft: true
tags: [PHP, Composer]
categories:
- [PHP, Composer]
---

使用composer安装composer.json文件中的依赖时，报错 `Allowed memory size of 1610612736 bytes exhausted (tried to allocate 83886080 bytes)`，反正就是说内存超出限制了。
实际是因为`php.ini`中 `memory_limit`的大小限制的。


### 修改php.ini
mac M1 通过`homebrew`安装的PHP，对应的`php.ini` 文件位置 `opt/homebrew/etc/php/7.4/php.ini`
我自己的环境修改为 2048M 后，composer 可以正常安装依赖。

```shell
memory_limit = 2048M
```
 
重新执行 composer install，开启愉悦地玩耍。

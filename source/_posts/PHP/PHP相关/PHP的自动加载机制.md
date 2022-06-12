---
title: "PHP的自动加载机制"
date: 2017-12-04 21:22:30
draft: true
tags: [PHP]
categories:
- [PHP]
---

#### 原因
我们在写PHP代码的时候，总会遇到这种情况（A.php需要引入XXX.php才能正常运行），代码结构比较小的话，通常我们都是直接通过 include 或者 require 直接引入，如果需要引入的文件不多的话，还可以接受，但是如果引入的文件达到几十个以上，再用 include 和 require 就显得比较繁琐，影响代码的美观。因此我们需要引入PHP文件的自动加载机制。

#### 方法一
使用 __autoload() 函数

```php
// __autoload — Attempt to load undefined class
// void __autoload ( string $class )

<?php
	function __autoload($class){
		require_once( $class.".php");
	}
?>


```
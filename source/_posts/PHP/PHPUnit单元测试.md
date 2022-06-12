---
title: "PHPUnit单元测试"
date: 2017-12-04 21:22:30
draft: true
tags: [PHP, PHPUnit]
categories:
- [PHP, PHPUnit]
---

###前言
单元测试这个问题对于新人来说总感觉没意思，感觉单元测试做不做都没啥问题，有时候还总觉得做单元测试很浪费时间。我总感觉 PHP 的开发者们并没有对 PHP 的质量有所追求，让大部分的开发者总以为浏览器访问就没有问题，所以很多时候，做 PHP 开发的，就没有单元测试的这些概念了。能不能有点追求？作为一个开发者，或者说是一个产品的经手人，就应该用心地去做好每个细节，一次比一次要更好。但是做单元测试，质量检查，是需要一定的时间和人力投入的，但我敢保证地说，你花时间投入的，绝对不会是没用的，一定对你，对项目来说，是一个质的提升，只要你肯投入时间用心去做。

单元测试是对单独的代码对象进行测试的过程，比如对函数、类、方法进行测试。单元测试可以使用任意一段已经写好的测试代码，也可以使用一些已经存在的测试框架，比如JUnit、PHPUnit或者Cantata++，单元测试框架提供了一系列共同、有用的功能来帮助人们编写自动化的检测单元，例如检查一个实际的值是否符合我们期望的值的断言。单元测试框架经常会包含每个测试的报告，以及给出你已经覆盖到的代码覆盖率。总之一句话，使用 phpunit 进行自动测试，会使你的代码更健壮，减少后期维护的成本，也是一种比较标准的规范。现如今流行的PHP框架都带了单元测试，如Laraval,Symfony,Yii2等，单元测试已经成了标配。另外，单元测试用例是通过命令操控测试脚本的，而不是通过浏览器访问URL的。

直接简单讲讲 PHPUnit吧。
官网：https://phpunit.de/
中文版官网：http://www.phpunit.cn/
###1.安装
习惯用 composer，使用 composer 安装吧。关于composer的安装可以参考https://www.phpcomposer.com/ 的介绍。

```
composer require --dev phpunit/phpunit ^6.5
//这里的 ^6.5 是版本号
```
###2.PHPunit的已引入

```
<?php

use PHPUnit\Framework\TestCase;

//类名不能以Test开头，否则无法运行
class DemoTest extends TestCase 
{
	//Todo
}
```
###3.输出格式的解读。

.	成功 
F	失败 
E 错误 
R 有风险 
S 跳过 
I 未完成

###4.常见的断言方法
详细的断言方法和参数详解可以参考PHPUnit中文网站的文档https://phpunit.readthedocs.io/zh_CN/latest/assertions.html

1.判断一个数组中是否含有指定的key
```
assertArrayHasKey()
```


2.判断是否含有数组形式的子集
```
assertArraySubset()
```


3.判断数组中是否含有指定的值

```
assertContains()
```
4.判断数组中不含指定的值，与3相反
```
assertNotContains()
```
5.判断数组中的数据是否仅是指定的数据类型，比如interger，float，boolean，object，string等。
```
assertContainsOnly()
```
6.与5相反，比如interger，float，boolean，object，string等
```
assertNotContainsOnly()
```
7.
```
assertContainsOnlyInstancesOf()
```
8.判断数组的长度和指定的数组长度是否相等
```
assertCount()
```
9.判断指定的多个数组的值是否相等，数组的元素顺序和值都会进行判断
```
assertEquals()
```

10.与9相反
```
assertNotEquals()
```


###5.字符串的常用断言方法
1.判断字符串中是否含有某个字符

```
assertContains()
```
2.判断字符串
```
assertEquals()
```
3.判断字符串与指定的正则表达式能否匹配
```
assertRegExp()
```
4.判断字符串
```
assertStringMatchesFormat()
```
5.判断字符串
```
assertStringMatchesFormatFile()
```
6.判断字符串与指定字符是否相等，assertSame()采用===判断，assertEquals()采用==判断
```
assertSame()
```
7.与6相反
```
assertNotSame()
```
8.判断字符串是否以指定的字符结尾
```
assertStringEndsWith()
```
9.判断字符串是否与指定的文件内容相等
```
assertStringEqualsFile()
```
10.判断字符串是否以指定的字符开头
```
assertStringStartsWith()
```
###6.类的断言方法
1.判断类中是否含有指定的某个属性，不管这个属性是否为static，public，private，protected都可以
```
assertClassHasAttribute()
```
2.判断类中是否含有静态属性，只判断是否有static修饰
```
assertClassHasStaticAttribute()
```
3.判断类是否相等，会判断类里面的值，单不判断数据类型，采用==判断
```
assertEquals()
```
4.判断一个类是否来自一个指定的继承类
```
assertInstanceOf()
```
5.断言一个类的实例是否含有指定的属性
```
assertObjectHasAttribute()
```
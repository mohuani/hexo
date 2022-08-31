---
title: "springboot启动失败常见问题"
date: 2016-06-17 21:22:30
draft: true
tags: [Java, Springboot]
categories:
- [Java, Springboot]
---

<!-- - 刚开始使用springboot连接mysql数据库，项目启动的时候报了一个错误 -->

```bash
Loading class `com.mysql.jdbc.Driver'. This is deprecated. The new driver class is `com.mysql.cj.jdbc.Driver'. The driver is automatically registered via the SPI and manual loading of the driver class is generally unnecessary.
```
原因：现在很多定的教学视频都比较老，教学使用的mysql版本和你自己本地使用的版本不一致，新版本的mysql需要设置成  “com.mysql.cj.jdbc.Driver”，另外还需要增加一个参数 serverTimezone=UTC
参考文档：https://blog.csdn.net/weixin_43770545/article/details/90486809

- 项目启动的时候报错，很显然数据库没有正常连接
```bash
java.sql.SQLException: Access denied for user ''@'localhost' (using password: NO)
```
我当时写的配置是，写的时候IDEA直接提示选了一个，单词没选对data-username 和 data-password，实际应该使用 username 和 password，主要还是没有记清楚配置。当作是一个教训吧。
错误的配置：
```yaml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/blog?useUnicode=true&charseterEncoding=utf-8&serverTimezone=UTC
    data-username: root
    data-password: root
```
正确的配置
```yaml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/blog?useUnicode=true&charseterEncoding=utf-8&serverTimezone=UTC
    username: root
    password: root
    
```



编译报错，忘记是什么报错了，解决方案先放这，后面遇到再补

```
-Djps.track.ap.dependencies=false
```

![image](https://user-images.githubusercontent.com/21000558/187575076-7a1e8873-c28c-4aed-855a-0a635a787af6.png)




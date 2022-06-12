---
title: "Java-注解"
date: 2016-06-15 21:22:30
draft: true
tags: [Java]
categories:
- [Java]
---

##### 参考视频
反射——全面解析Java注解 https://www.imooc.com/learn/456

------
#### Java中的常见注解

JDK自带注解

- @Override，表示当前的方法定义将覆盖超类中的方法。

- @Deprecated，使用了注解为它的元素编译器将发出警告，过时被弃用的代码，不建议再使用。

- @SuppressWarnings，关闭不当编译器警告信息。

    例子：@SuppressWarnings("deprecation")，仍然使用已经不推荐的方法

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190409151031923.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)

#### 注解分类
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190409151116246.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190409151127566.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
#### 自定义注解
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190409151219170.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190409151719465.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190409151905183.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)


![在这里插入图片描述](https://img-blog.csdnimg.cn/20190409152012363.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190409152127111.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190409152211808.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
#### 注解应用实战
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190409152502113.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190409161122387.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)


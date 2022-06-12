---
title: "TensorFlow学习笔记4"
date: 2019-09-03 19:36:38
draft: true
tags: [计算机相关, TensorFlow]
categories:
- [计算机相关, TensorFlow]
---

**placeholder传值**

```
import tensorflow as tf

input1 = tf.placeholder(tf.float32)
input2 = tf.placeholder(tf.float32)

output = tf.multiply(input1,input2)

# with tf.Session as sess:
sess = tf.Session()
print(sess.run(output,feed_dict={input1:[7.],input2:[2.]}))
```
运行的结果

```
[14.]
```
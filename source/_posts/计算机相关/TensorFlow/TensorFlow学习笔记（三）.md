---
title: "TensorFlow学习笔记3"
date: 2019-09-03 19:36:38
draft: true
tags: [计算机相关, TensorFlow]
categories:
- [计算机相关, TensorFlow]
---

**Variable变量的简单使用**

在 Tensorflow 中，定义了某字符串是变量，它才是变量，这一点是与 Python 所不同的

```
import tensorflow as tf

state = tf.Variable(0,name = 'counter')
print(state.name)

one = tf.constant(1)

new_vaule = tf.add(state,one)
update = tf.assign(state,new_vaule)

# 2018-4-6记录
# 莫烦的视频中用的是老版本 initialize_all_variables()
# 现在新版的函数是 global_variables_initializer()
# init = tf.initialize_all_variables()
init = tf.global_variables_initializer()


sess = tf.Session()
sess.run(init)
for _ in range(3):
    sess.run(update)
    print(sess.run(state))
sess.close()
```

运行的结果

```
counter:0

1
2
3
```
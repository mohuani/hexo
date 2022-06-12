---
title: "TensorFlow学习笔记2"
date: 2019-09-03 19:36:38
draft: true
tags: [计算机相关, TensorFlow]
categories:
- [计算机相关, TensorFlow]
---

**Session的两种使用方式**

```
import tensorflow as tf

matrix1 = tf.constant([[3,3]])
matrix2 = tf.constant([[2,2]])

product = tf.matmul(matrix1,matrix2)  # matrix multiply  np.dot(m1,m2)

# session的两种打开模式

# method 1

# sess = tf.Session()
# result1 = sess.run(product)
# print(result1)
# sess.close()

# method 2
with tf.Session() as sess:
    result2 = sess.run(product)
    print(result2)
```

运行的结果

```
[[12]]
```
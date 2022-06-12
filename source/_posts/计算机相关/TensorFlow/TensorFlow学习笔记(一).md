---
title: "TensorFlow学习笔记1"
date: 2019-09-03 19:36:38
draft: true
tags: [计算机相关, TensorFlow]
categories:
- [计算机相关, TensorFlow]
---

自己构建一些数据，来实现一个简单函数的模拟学习

```
import tensorflow as tf
import numpy as np

# create data
x_data = np.random.rand(100).astype(np.float32)
y_data = x_data*0.1 + 0.3

# create tensorflow structure start
Weights = tf.Variable(tf.random_uniform([1],-1,0))
biases = tf.Variable(tf.zeros([1]))

y = Weights*x_data + biases

loss = tf.reduce_mean(tf.square(y-y_data))
optimizer = tf.train.GradientDescentOptimizer(0.5)
train = optimizer.minimize(loss)

# 2018-4-6记录
# 莫烦的视频中用的是老版本 initialize_all_variables()
# 现在新版的函数是 global_variables_initializer()
# init = tf.initialize_all_variables()

init = tf.global_variables_initializer()

# create tensorflow structure end

sess = tf.Session()
sess.run(init)

for step in range(401):
    sess.run(train)
    if step % 20 == 0:
        print(step,sess.run(Weights),sess.run(biases))


```

经过401的迭代后输出的结果
```
0 [-0.39455852] [0.70643663]
20 [-0.05729461] [0.37692836]
40 [0.05577959] [0.32162696]
60 [0.08756826] [0.30608]
80 [0.09650504] [0.3017093]
100 [0.09901748] [0.30048054]
120 [0.09972379] [0.3001351]
140 [0.09992234] [0.300038]
160 [0.09997816] [0.30001068]
180 [0.09999388] [0.30000302]
200 [0.09999828] [0.30000085]
220 [0.09999953] [0.30000025]
240 [0.09999986] [0.30000007]
260 [0.0999999] [0.30000007]
280 [0.0999999] [0.30000007]
300 [0.0999999] [0.30000007]
320 [0.0999999] [0.30000007]
340 [0.0999999] [0.30000007]
360 [0.0999999] [0.30000007]
380 [0.0999999] [0.30000007]
400 [0.0999999] [0.30000007]
```
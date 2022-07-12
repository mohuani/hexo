---
title: "第6章-领略SET化架构衍化与设计"
date: 2022-07-12 11:36:39
draft: true
tags: [数据库, RabbitMQ]
categories:
- [数据库, RabbitMQ]
---


### 6-1 本章导航：
- 了解set化架构的进衍
- 互联网大厂是如何进行set化的
- set化架构的设计和解决方案
- rabbitMQ SET化架构的搭建

### 6-2 BAT、TMD大厂单元化架构设计衍变之路分享
![image](https://user-images.githubusercontent.com/21000558/178398964-46f40e48-fa8e-4565-90d1-61ebeeb77679.png)

![image](https://user-images.githubusercontent.com/21000558/178399093-a005ac8c-0eb2-4003-91b9-78a75a9a013f.png)

### 6-3 SET化架构设计策略(异地多活架构)

![image](https://user-images.githubusercontent.com/21000558/178399212-fb567b88-148c-4164-ad2f-773cb949ca66.png)


### 6-4 SET化架构设计原则

#### 对业务透明原则：
  - set化架构的实现对业务代码透明，业务代码层面不需要关心SET化规则，SET的部署等问题。
#### SET化切分的规则
  - 理论上，切分规则由业务层面按需定制；
  - 实现上，建议优先选最大的业务维度进行切分。
  - 比如海量用户的o2o业务，按用户位置信息进行切分。 此外，接入层，逻辑层和数据层可以有独立的SET切分规则，有利于实现部署和运维成本的最优化。
#### 部署规范原则
  - 一个SET并不一定只限制在一个机房，也可以跨机房或者跨地区部署；为保证灵活性，单个SET内机器数不宜过多（如不超过100台物理机)

### 6-5 SET化消息中间件架构实现

![image](https://user-images.githubusercontent.com/21000558/178400007-637f6a17-0697-4951-b763-313aaf76f713.png)

![image](https://user-images.githubusercontent.com/21000558/178400283-51fe1c32-719f-4281-b049-34d9f7eadb86.png)

![image](https://user-images.githubusercontent.com/21000558/178400593-768ba5bb-7814-4c9a-a157-190b00fdedd4.png)

![image](https://user-images.githubusercontent.com/21000558/178400667-c12db431-5e59-4757-9abf-eec1cf96a5ea.png)

![image](https://user-images.githubusercontent.com/21000558/178400698-3a624749-70f2-403c-b42c-3fbf5fce8b4d.png)

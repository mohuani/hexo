---
title: "ElasticStack3"
date: 2021-10-05 17:36:39
draft: true
tags: [数据库, ElasticStack]
categories:
- [数据库, ElasticStack]
---

### 第3章 Elasticsearch 篇之倒排索引与分词
#### 3-1 -书的目录与索引
![image](https://user-images.githubusercontent.com/21000558/135994660-414acd7d-6187-4b45-8888-39e16a8e290f.png)

#### 3-2 -正排与倒排索引简介
![image](https://user-images.githubusercontent.com/21000558/135995069-72e826ba-ddc1-4188-a3dd-5818d38b482e.png)
![image](https://user-images.githubusercontent.com/21000558/135995119-284c4f3c-ada4-4c74-bb40-14d3ed3e8f08.png)
![image](https://user-images.githubusercontent.com/21000558/135995141-80181628-d67e-4392-8dec-0dfe12a699be.png)
![image](https://user-images.githubusercontent.com/21000558/135995018-1206078e-cc32-409b-be02-9a32e4b6bca0.png)

#### 3-3 -倒排索引详解 
![image](https://user-images.githubusercontent.com/21000558/135995388-95357d71-26d9-413d-ae4c-cd0a4c9b7a26.png)
![image](https://user-images.githubusercontent.com/21000558/135995412-8b650aeb-6cb1-476a-882d-79d6aaff2ba0.png)
![image](https://user-images.githubusercontent.com/21000558/135995665-f26ccc51-f26a-4625-9585-1abfedf08322.png)
![image](https://user-images.githubusercontent.com/21000558/135996594-d4c5b8af-1614-4fcc-8218-42ba0fc5c468.png)
![image](https://user-images.githubusercontent.com/21000558/135996804-3903197e-d0df-4dea-95ac-ab6325af11c4.png)
![image](https://user-images.githubusercontent.com/21000558/135996995-23194031-d23f-496c-aaeb-451af12a5056.png)

#### 3-4 -分词介绍
![image](https://user-images.githubusercontent.com/21000558/135997099-10797f5a-787f-40a4-b4fa-07062d528ada.png)
![image](https://user-images.githubusercontent.com/21000558/135997486-3ca4cbee-d4ca-48ee-911a-dd1e964884ee.png)


#### 3-5 -analyze_api 
![image](https://user-images.githubusercontent.com/21000558/135997720-a993095f-3083-4005-9d62-3c4127d02cc9.png)
![image](https://user-images.githubusercontent.com/21000558/135997750-e565a18f-bea5-41c7-9ac0-f48a6abc2d91.png)
![image](https://user-images.githubusercontent.com/21000558/135998239-32e922e1-2218-41b6-9589-eef049653f1b.png)

![image](https://user-images.githubusercontent.com/21000558/135998185-0cf94c2e-236c-451a-9624-b7937b2771ce.png)

#### 3-6 -自带分词器 
![image](https://user-images.githubusercontent.com/21000558/135998724-6cd5547b-a9fa-4029-8109-39ccddde75d8.png)
standard
![image](https://user-images.githubusercontent.com/21000558/135998757-f6045823-4169-4449-a768-c8f3515667a7.png)
sample
![image](https://user-images.githubusercontent.com/21000558/135999022-542fe139-0727-4b66-a3e4-a4fd52aaa17f.png)
whitespace
![image](https://user-images.githubusercontent.com/21000558/136001127-0f0a99ea-4156-4174-a852-a2d65e232097.png)
stop

![image](https://user-images.githubusercontent.com/21000558/136001282-e5977ac6-b5c2-43fe-86fa-69baa2550284.png)
keyword
![image](https://user-images.githubusercontent.com/21000558/136001369-dbc8f9c6-7a35-4059-bd44-c0c8b8a5df9e.png)
pattern

![image](https://user-images.githubusercontent.com/21000558/136001563-5f6d1dcb-c9f2-4d8a-9a10-ee2ef7136f9d.png)
language
![image](https://user-images.githubusercontent.com/21000558/136001773-ef03027d-0c03-4f12-8dc3-c0fcc9966f7f.png)



#### 3-7 -中文分词 
[一篇文章总结语言处理中的分词问题](https://mp.weixin.qq.com/s?__biz=MzU1NDA4NjU2MA==&mid=2247486148&idx=1&sn=817027a204650763c1bea3e837d695ea)
![image](https://user-images.githubusercontent.com/21000558/136002601-8fecc6ee-b445-4ec0-98df-df992999cd2e.png)
![image](https://user-images.githubusercontent.com/21000558/136002694-41a7884c-f6ba-4ed3-9672-2f659331a718.png)


#### 3-8 -自定义分词之CharacterFilter 
![image](https://user-images.githubusercontent.com/21000558/136002867-e19f860b-cdb2-4dfe-9151-ccef034d477c.png)
![image](https://user-images.githubusercontent.com/21000558/136002933-a77ce256-a81b-403c-ad9c-328a640fca42.png)


#### 3-9 -自定义分词之Tokenizer 
![image](https://user-images.githubusercontent.com/21000558/136003121-55a9fa14-767b-49f5-9b02-7756303a049d.png)

#### 3-10 -自定义分词之 TokenFilter
![image](https://user-images.githubusercontent.com/21000558/136003371-1bf98c2c-1e1b-412d-8f99-1bc02794db6d.png)
![image](https://user-images.githubusercontent.com/21000558/136003420-727adacb-7c1e-4c92-b1b3-27064e0648f6.png)

#### 3-11 -自定义分词
![image](https://user-images.githubusercontent.com/21000558/136004523-a22bac80-1a12-4077-ab41-1a980f5d74b5.png)
![image](https://user-images.githubusercontent.com/21000558/136004624-58f542d0-81f7-4f54-8a27-b77bc137dfa7.png)
![image](https://user-images.githubusercontent.com/21000558/136004666-4f1e57ed-48ba-46d7-9894-574aa770b6a0.png)
![image](https://user-images.githubusercontent.com/21000558/136004819-22192cef-6e2f-4d51-9f7d-968ddd9b0b42.png)

#### 3-12 -分词使用说明
![image](https://user-images.githubusercontent.com/21000558/136005159-bbc0051d-ee49-45e7-9857-5bc9c3f3a1e1.png)
![image](https://user-images.githubusercontent.com/21000558/136005180-4a382c72-41f4-4902-ba1d-ad8a80316d72.png)
![image](https://user-images.githubusercontent.com/21000558/136005236-57373e03-b4d5-484e-803c-d81bdd902b15.png)
![image](https://user-images.githubusercontent.com/21000558/136005393-9dcf1cc5-68a2-48db-b801-be6e5de415af.png)


#### 3-13 -官方文档说明 



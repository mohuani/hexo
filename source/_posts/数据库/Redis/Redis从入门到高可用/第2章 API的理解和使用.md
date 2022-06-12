---
title: "Redis初识"
date: 2022-03-31 21:36:39
draft: true
tags: [数据库, Redis]
categories:
- [数据库, Redis]
---

### 2-2通用命令

```
keys * #遍历所有key
del key [key…] #删除指定的key-value
dbsizee 算出key的总数
expire key seconds #key在sc秒后过期
ttl key #查看key剩余的过期时间（-1 代表没有过期时间 -2代表已过期）
persist key #去掉key的过期时间
exists key #检查一个key是否存在
type key #返回key的类型,结果为string hash list set zset none（key不存在）
```
![image](https://user-images.githubusercontent.com/21000558/161048110-cf898bdd-7782-41d6-8643-b479a673fc0f.png)

### 2-3 数据结构和内部编码

![1648653844115-f9099d9c-0908-4971-a374-08e3d655e908](https://user-images.githubusercontent.com/21000558/161048234-0f018813-f074-4ca2-a690-6979a84044ce.png)


![image](https://user-images.githubusercontent.com/21000558/161047779-7cabd544-76b2-4edd-ac01-624cd9cf63a5.png)

### 2-4 单线程

![image.png](https://cdn.nlark.com/yuque/0/2022/png/85934/1648654084762-d87447aa-47bf-4ce9-b699-68d714981d89.png#clientId=u2e23ff42-1b97-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u8a3010f8&margin=%5Bobject%20Object%5D&name=image.png&originHeight=334&originWidth=1721&originalType=url&ratio=1&rotation=0&showTitle=false&size=160226&status=done&style=none&taskId=u098303d1-66bd-45de-8015-b85faa44735&title=)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/85934/1648654093328-eab0b695-d1fb-4ca9-9505-b783cb4a9421.png#clientId=u2e23ff42-1b97-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u75a27ea0&margin=%5Bobject%20Object%5D&name=image.png&originHeight=685&originWidth=1474&originalType=url&ratio=1&rotation=0&showTitle=false&size=373801&status=done&style=none&taskId=ufb74172d-7162-42bd-802e-e87a37295e3&title=)

单线程为什么还这么快？

- 纯内存
- 非阻塞IO
- 避免线程切换和竞态消耗

其它注意点

- 一次只运行一条命令
- 拒绝长（慢）命令 

```
keys，flushall，flushdb，slow lua script，mutil/exec，operate big value（collection）
```

- 其实不是全是单线程，如下面的命令  `fysnc file descriptor `, `close file descriptor`


### 2-5 字符串

场景： 缓存， 计数器，分布式锁

#### 特点

key都为字符串 value可为 字符串（json） 整数 二进制

API

```bash
注:以下命令 时间复杂度均为 o(1)


get key  
#获取key对应的value

set key value
#设置key-value

del key 
#删除key-value

incr key
#key自增1，如果key不存在，自增后get(key)=1

decr key o（1）
#key自减1，如果key不存在，自减后get（key）=-1

incrby key k
#Key自增k，如果key不存在自增后get(key）=k 

decr key k
#Key自减k，如果key不存在，自减后get（key）=-k 

set key value
#不管key是否存在，都设置

setnx key value
#key不存在，才设置

set key value xx
#key存在，才设置

```

```bash
注:以下命令 时间复杂度均为 o(n)

mget keyl key2 key3...
#批量获取key，原子操作

mset key1 valuel key2 value2 key3 value3
#批量设置key-value
```

```bash
注:以下命令 时间复杂度均为 o(1)

getset key newvalue
#set key newvalue并返回旧的value

append key value
#将value追加到旧的value（相当于字符串拼接）

strlen key
#返回字符串的长度（注意中文）

incrbyfloat key 3.5
#增加key对应的值3.5

getrange key start end
#获取字符串指定下标所有的值

setrange key index value
#设置指定下标所有对应的值

```

### 2.5 hash
特点
key还是String类型 ，值则分成 field和value，可以添加field来对属性进行扩充
redis本身就是一个map结构的，所以可以把这种数据结构理解成 “mapmap”,其中 field就像key一样不能相同 。

![image](https://user-images.githubusercontent.com/21000558/161049034-d89f9bc9-0e28-4afe-9484-76f7111e48bc.png)

API所有命令都是以h开头

```
命令 时间复杂度均为 o(1)

hget key field
#获取hash key对应的field的value

hset key field value
#设置hash key对应field的value

hdel key field
#删除hash key对应field的value

hexists key field
#判断hash key是否有field

hlen key
#获取hash key field的数量

hsetnx key field value
#设置hash key对应field的value（如field已经存在，则失败）

hincrby key field intCounter
#hash key对应的field的value自增intCounter

hincrbyfloat key field floatCounter
#hincrby浮点数版
```

```
命令 时间复杂度均为 o(n)

hmget key field1 field2....fieldN
#批量获取hash key的一批field对应的值

hmset key fieldI valuel field2 value2..fieldN valueN
#批量设置hash key的一批field value

hgetall key
#返回hash key对应所有的field和value

hvals key
#返回hash key对应所有field的value

hkeys key
#返回hash key对应所有field

```

![image](https://user-images.githubusercontent.com/21000558/161050160-727a81a6-5504-47bd-9ea2-2271106007c3.png)
![image](https://user-images.githubusercontent.com/21000558/161050319-e686a3c5-1f30-4a87-b6c6-043f50541be2.png)















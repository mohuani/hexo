---
title: "Redis跳跃表"
date: 2022-03-16 20:36:39
draft: true
tags: [数据库, Redis, Redis跳跃表]
categories:
- [数据库, Redis, Redis跳跃表]
---

**惯例参考文章放上面**
- [深入理解Redis跳跃表的基本实现和特性](https://juejin.cn/post/6893072817206591496)
- [面试准备 -- Redis 跳跃表](https://blog.csdn.net/weixin_41622183/article/details/91126155)
- [Redis(2)——跳跃表](https://segmentfault.com/a/1190000022028505)

> Redis跳跃表是已经容易被问到的问题




### 跳跃表的构成

![image](https://user-images.githubusercontent.com/21000558/158731849-938179e2-8e31-4213-ad2a-d6e0a719f572.png)

从图中可以看到， 跳跃表主要由以下部分构成：

- `表头（head）`：负责维护跳跃表的节点指针。
- `跳跃表节点`：保存着元素值，以及多个层。
- `层`：保存着指向其他元素的指针。高层的指针越过的元素数量大于等于低层的指针，为了提高查找的效率，程序总是从高层先开始访问，然后随着元素值范围的缩小，慢慢降低层次。
- `表尾`：全部由 NULL 组成，表示跳跃表的末尾。

### Redis 跳跃表的实现

Redis 的跳跃表由 `redis.h/zskiplistNode` 和 `redis.h/zskiplist` 两个结构定义， 其中 `zskiplistNode` 结构用于表示跳跃表节点， 而 `zskiplist` 结构则用于保存跳跃表节点的相关信息， 比如节点的数量， 以及指向表头节点和表尾节点的指针， 等等。

![image](https://user-images.githubusercontent.com/21000558/158732032-5a3c19ac-695d-4f82-994d-007e01ae19a7.png)

上图展示了一个跳跃表示例，位于图片最左边的示 zskiplist 结构，该结构包含以下属性：

`header` ：指向跳跃表的表头节点。
`tail` ：指向跳跃表的表尾节点。
`level` ：记录目前跳跃表内，层数最大的那个节点的层数（表头节点的层数不计算在内）。
`length` ：记录跳跃表的长度，也即是，跳跃表目前包含节点的数量（表头节点不计算在内）。

位于 `zskiplist` 结构右方的是四个 `zskiplistNode` 结构， 该结构包含以下属性：

- `层（level）`：节点中用 L1 、 L2 、 L3 等字样标记节点的各个层， L1 代表第一层， L2 代表第二层，以此类推。每个层都带有两个属性：前进指针和跨度。前进指针用于访问位于表尾方向的其他节点，而跨度则记录了前进指针所指向节点和当前节点的距离。在上面的图片中，连线上带有数字的箭头就代表前进指针，而那个数字就是跨度。当程序从表头向表尾进行遍历时，访问会沿着层的前进指针进行。
- `后退（backward）指针`：节点中用 BW 字样标记节点的后退指针，它指向位于当前节点的前一个节点。后退指针在程序从表尾向表头遍历时使用。
- `分值（score）`：各个节点中的 1.0 、 2.0 和 3.0 是节点所保存的分值。在跳跃表中，节点按各自所保存的分值从小到大排列。
- `成员对象（obj）`：各个节点中的 o1 、 o2 和 o3 是节点所保存的成员对象。

注意：表头节点和其他节点的构造是一样的： 表头节点也有后退指针、分值和成员对象， 不过表头节点的这些属性都不会被用到， 所以图中省略了这些部分， 只显示了表头节点的各个层。



Redis的跳跃表实现是由redis.h/zskiplistNode和redis.h/zskiplist（3.2版本之后redis.h改为了server.h）两个结构定义，zskiplistNode定义跳跃表的节点，zskiplist保存跳跃表节点的相关信息

```ccp
/* ZSETs use a specialized version of Skiplists */
typedef struct zskiplistNode {
    // 成员对象 （robj *obj;）
    sds ele;
    // 分值
    double score;
    // 后退指针
    struct zskiplistNode *backward;
    // 层
    struct zskiplistLevel {
        // 前进指针
        struct zskiplistNode *forward;
        // 跨度
        // 跨度实际上是用来计算元素排名(rank)的，在查找某个节点的过程中，将沿途访过的所有层的跨度累积起来，得到的结果就是目标节点在跳跃表中的排位
        unsigned long span;
    } level[];
} zskiplistNode;

typedef struct zskiplist {
    // 表头节点和表尾节点
    struct zskiplistNode *header, *tail;
    // 表中节点的数量
    unsigned long length;
    // 表中层数最大的节点的层数
    int level;
} zskiplist;

```
#### zskiplistNode结构

- level数组（层）：每次创建一个新的跳表节点都会根据幂次定律计算出level数组的大小，也就是次层的高度，每一层带有两个属性-前进指针和跨度，前进指针用于访问表尾方向的其他指针；跨度用于记录当前节点与前进指针所指节点的距离（指向的为NULL，阔度为0）
- backward（后退指针）：指向当前节点的前一个节点
- score（分值）：用来排序，如果分值相同看成员变量在字典序大小排序，数据类型是double
- obj或ele：成员对象是一个指针，指向一个字符串对象，里面保存着一个sds；在跳表中各个节点的成员对象必须唯一，分值可以相同

#### zskiplist结构

- header、tail表头节点和表尾节点
- length表中节点的数量
- level表中层数最大的节点的层数


### 总结

- 多层的结构组成，每层是一个有序的链表
- 跳跃表是有序集合的底层实现之一， 除此之外它在 Redis 中没有其他应用。
- Redis 的跳跃表实现由 zskiplist 和 zskiplistNode 两个结构组成， 其中 zskiplist 用于保存跳跃表信息（比如表头节点、表尾节点、长度）， 而 zskiplistNode 则用于表示跳跃表节点。
- 每个跳跃表节点的层高都是 1 至 32 之间的随机数。
- 在同一个跳跃表中， 多个节点可以包含相同的分值， 但每个节点的成员对象必须是唯一的。
- 跳跃表中的节点按照分值大小进行排序， 当分值相同时， 节点按照成员对象的大小进行排序。





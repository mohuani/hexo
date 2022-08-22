---
title: "Java Collection Examples"
date: 2022-08-18 21:22:30
draft: true
tags: [Java, Collection]
categories:
- [Java, Collection]
---


> 在使用PHP的Laravel框架时候，我们通常可以很容易地表达一些业务， keyBy("a")：比如将一个map按照字段a排序。pluck("a")：根据指定key获取对应的value然后组装成一个list， 数据去重 unique()。那么这些方法在Java里面应该怎么写呢

### 测试数据的数据结构
```java
public class Teacher {

    private Integer id;

    private String name;

    private List<Integer> studentIdList;
}
```


### 常见的方法
- `isEmpty`  `isNotEmpty`
- `keyBy`
- `list to map`
- `map to list`
- `groupBy`
- `unique`
- `min`，`max`
- `sum`
- `first`

### 实践
项目代码地址 [Java-Collection-Examples](https://github.com/mohuani/Java-Collection-Examples)
所有的调试代码都放在了 `JavaCollectionExamplesApplication`, 直接运行，自己测试一下就可以






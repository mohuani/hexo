---
title: "MySQL更新Json不生效"
date: 2021-04-15 15:36:39
draft: true
tags: [数据库, MySQL, Json]
categories:
- [数据库, MySQL, Json]
---

> MySQL5.7之后支持了json操作，我们可以很方便的使用相关的函数，通过原生 `sql`直接操作数据库，现在有一个需求：在`tableA`中字段`info`的类型为`json`，但是历史数据里面`info`有一部分的值是 `null`，有一部分的值是其他的 `{"key1": "value1"}`，选在需要给全部的`info`字段，如果里面有`key3`,那么值修改改为`value3`，如果里面没有`key3`，那么直接追加上 `"key3": "value3"`，

通常我们认为直接通过下面的sql语句就可以解决

```sql
update tableA set info = json_set(info, '$.key3', "value3") where id = 1;
```
但是在实际操作过程中，发现如果历史数据里面如果 `info`字段的值是 `null`的话，`json_set`是没有生效的，`info`字段有值的话，`json_set`才会生效，那么就需要下面的操作才能完成上面的需求

```sql
update tableA set info = '{"value3": "value3"}' where id = 1 and info is null ;
update tableA set info = json_set(info, '$.key3', "value3") where id = 1 and info is not null;
```

另外附上一篇文章：https://blog.csdn.net/xc_zhou/article/details/83031343

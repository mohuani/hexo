---
title: "mysql查看建表的注释"
date: 2019-10-18 20:36:39
draft: true
tags: [数据库, MySQL]
categories:
- [数据库, MySQL]
---

查看表结构-表格形式，带注释

```shell
show full columns from tableName;
```


查看表结构-建表语句，带注释

```shell
show create table tableName;
```

简单查看表结构

```shell
 desc tableName;
```


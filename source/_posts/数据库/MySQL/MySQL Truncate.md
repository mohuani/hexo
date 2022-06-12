---
title: "MySQL Truncate"
date: 2021-09-15 17:36:39
draft: true
tags: [数据库, MySQL]
categories:
- [数据库, MySQL]
---

### 1.truncate使用语法
`truncate`的作用是清空表或者说是截断表，只能作用于表。truncate的语法很简单，后面直接跟表名即可，例如： `truncate table table_name` 或者 `truncate table_name`。

执行`truncate`语句需要拥有表的`drop`权限，从逻辑上讲，`truncate table`类似于`delete`删除所有行的语句或`drop table`然后再`create table`语句的组合。

### 2.truncate与drop,delete的对比

- `truncate`与`drop`是DDL语句，执行后无法回滚；`delete`是DML语句，可回滚。
- `truncate`只能作用于表；`delete，drop`可作用于表、视图等。
- `truncate`会清空表中的所有行，但表结构及其约束、索引等保持不变；drop会删除表的结构及其所依赖的约束、索引等。
- `truncate`会重置表的自增值；`delete`不会。
- `truncate`不会激活与表有关的删除触发器；`delete`可以。
- `truncate`后会使表和索引所占用的空间会恢复到初始大小；`delete`操作不会减少表或索引所占用的空间，drop语句将表所占用的空间全释放掉。
### 3.truncate使用场景及注意事项
通过前面介绍，我们很容易得出`truncate`语句的使用场景，即该表数据完全不需要时可以用`truncate`。如果想删除部分数据用`delete`，注意带上`where`子句；如果想删除表，当然用`drop`；如果想保留表而将所有数据删除且和事务无关，用`truncate`即可；如果和事务有关，或者想触发`trigger`，还是用`delete`；如果是整理表内部的碎片，可以用`truncate`然后再重新插入数据。truncate表都是高危操作，特别是在生产环境要更加小心

#### 注意事项
- truncate无法通过binlog回滚。
- truncate会清空所有数据且执行速度很快。
- truncate不能对有外键约束引用的表使用。
- 执行truncate需要drop权限，不建议给账号drop权限。
- 执行truncate前一定要再三检查确认，最好提前备份下表数据。


### 4.table 被删除部分，但是id又想接着自增

tableA的自增id从 1-100，现在通过delete 删除了id在 51-100 的数据，通常后面添加的数据id会从101开始，但是我们有个需求希望id从51开始自增，那么我们可以重新设置自增id

```shell
alter table table_name auto_increment = 51;
```






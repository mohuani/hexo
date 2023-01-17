---
title: "Specified key was too long; max key length is 767 bytes"
date: 2023-01-17 20:36:39
draft: true
tags: [数据库, MySQL]
categories:
- [数据库, MySQL]
---

### 背景
> 从线上复制了一张表的表结构，打算在本地把该表创建出来，但是在执行时出现报错。之前这个报错也遇到过解决过，但是一段时间就忘记解决方案了，写篇文档记录下。


### 现象
MySQL版本：DBMS: MySQL (ver. 5.7.31-log)

表结构DDL

```mysql
DROP table IF EXISTS t_record_log;
create table t_record_log
(
    `id`          BIGINT AUTO_INCREMENT NOT NULL COMMENT '自增ID',
    `type`        VARCHAR(128)          NOT NULL DEFAULT '' COMMENT '类型',
    `code`        VARCHAR(255)          NOT NULL DEFAULT '' COMMENT '标识',
    `source_from` VARCHAR(255)          NOT NULL DEFAULT '' COMMENT '来源',
    `status`      int(1)                NOT NULL DEFAULT 0,
    PRIMARY KEY (id),
    UNIQUE KEY uniq_source_type_code (`source_from`, `type`, `code`)
) ENGINE = InnoDB
  AUTO_INCREMENT = 1
  DEFAULT CHARSET = utf8mb4
  COLLATE utf8mb4_bin COMMENT '记录log';

```

执行后报错

```shell
[2023-01-17 15:29:31] [42000][1071] Specified key was too long; max key length is 767 bytes
```

### 报错原因
https://dev.mysql.com/doc/refman/5.7/en/charset-unicode-utf8mb4.html
1、库字符集COLLATE=utf8mb4_bin， 该表使用的字符集是COLLATE=utf8mb4_bin， CHARSET=utf8mb4，[MySQL5.7官方文档说明](https://dev.mysql.com/doc/refman/5.7/en/charset-unicode-utf8mb4.html) utf8mb4 使用4个字节，所以做索引的字段的做大长度应该是  767 / 4 = 191.75  ->  191，需要对上表的字段code和字段source_from修改一下长度。

```mysql
create table t_record_log
(
    `id`          BIGINT AUTO_INCREMENT NOT NULL COMMENT '自增ID',
    `type`        VARCHAR(128)          NOT NULL DEFAULT '' COMMENT '类型',
    `code`        VARCHAR(100)          NOT NULL DEFAULT '' COMMENT '标识',
    `source_from` VARCHAR(191)          NOT NULL DEFAULT '' COMMENT '来源',
    `status`      int(1)                NOT NULL DEFAULT 0,
    PRIMARY KEY (id),
    UNIQUE KEY uniq_source_type_code (`source_from`, `type`, `code`)
) ENGINE = InnoDB
  AUTO_INCREMENT = 1
  DEFAULT CHARSET = utf8mb4
  COLLATE utf8mb4_bin COMMENT '记录log';

```
DDL修改后，执行成功


#### 拓展命令

```shell
show engines;

show character set;

show collation;
```

### 参考文章
- https://help.aliyun.com/document_detail/41707.html
- https://stackoverflow.com/questions/1814532/mysql-error-1071-specified-key-was-too-long-max-key-length-is-767-bytes















---
title: "MySQL数据库性能分析和优化"
date: 2019-10-18 20:36:39
draft: true
tags: [数据库, MySQL]
categories:
- [数据库, MySQL]
---

课程地址：https://www.imooc.com/learn/194

推荐文章：https://www.zam9.com/blog/mysql_opt01

### MySQL数据库优化

#### SQL语句优化

**慢查询**

如何发现有问题的SQL？

使用mysql慢查询日志对有效率问题的SQL进行监控

```
//查看慢查询日志是否开启
show variables like 'slow_query_log';

//查看慢查询日志存储位置
show variables like 'slow_query_log_file';

//开启慢查询日志
set global slow_query_log=on;

//指定慢查询日志存储位置
set global show_query_log_file='/var/lib/mysql/homestead-slow.log';

//记录没有使用索引的sql
set global log_queries_not_using_indexes=on;

//记录查询超过1s的sql
set global long_query_time=1;
```

慢查询日志所包含的内容：

```
#User@Host:root[root] @localhost[]//执行sql的主机信息
#Query_time:0.0000024 Lock_time:0.00 Rows_sent:0 Rows_esamined:0//sql的执行信息
SET timestamp=1402389324//sql执行时间
select * from store; //sql的内容
```

**MySQL慢查询日志分析工具之mysqldumpslow（mysql官方）**

安装完MySQL后，默认就带了mysqldumpslow，很常用的一个工具。

```
//查看参数列表
mysqldumpslow -h

//分析慢查询日志中前三条比较慢的sql
mysqldumpslow -t 3 /var/lib/mysql/homestead-slow.log | more 

//输出样式效果
Count:1 Time:0.00s Lock=0.00s Rows=10.0
root[rppt]@localhost
select * from store
```

**MySQL慢查询日志分析工具之pt-query-digest**

分析结果比mysqldumpslow更详细全面

```
//输出到文件
pt-query-digest slow-log > slow_log.report

//输出到数据表
pt-query-digest slow.log -review \
	h=127.0.0.1,D=test,p=root,P=3306,u=root,t=query_review \
	--create-reviewtable \
	--review-history t=hostname_slow
```

基本使用

```
//查看参数列表
pt-query-digest --help

//分析慢查询日志中前三条比较慢的sql
pt-query-digest /var/lib/mysql/homestead-slow.log | more 

//输出分为三部分
1.显示除了日志的时间范围，以及总的sql数量和不同的sql数量
2.Response Time:响应时间占比 Calls:sql执行次数
3.sql的具体日志
```

如何通过慢查询日志发现有问题的SQL？

```
1.查询次数多且每次查询占用时间长的SQL
通常为pt-query-digest分析的前几个查询

2.IO大的SQL（数据库主要瓶颈出现在IO层次）
注意pt-query-digest分析中的Rows examine项

3.未命中索引的SQL
注意pt-query-digest分析中的Rows examine和Rows Send的对比
```

通过explain查询和分析SQL的执行计划

```
explain select customer_id,,first_name,last_name from customer;
```

| id   | select_type | table              | type                                                         | possible_keys                              | key                                | key_len                                  | ref                              | rows                            | Extra                               |
| ---- | ----------- | ------------------ | ------------------------------------------------------------ | ------------------------------------------ | ---------------------------------- | ---------------------------------------- | -------------------------------- | ------------------------------- | ----------------------------------- |
| 1    | SIMPLE      | customer           | ALL                                                          | NULL                                       | NULL                               | NULL                                     | NULL                             | 671                             |                                     |
|      |             | 该数据关于哪张表。 | 示连接使用了何种类型。从好到差const,eq_reg,ref,range,index和ALL。 | 可能应用在该表的索引，空，没有可能的索引。 | 实际使用的索引。空，没有使用索引。 | 使用的索引长度。不损失精度下，越短越好。 | 显示索引的哪一列被使用了，常数。 | mysql认为必须检查的数据的行数。 | 注意：Using filesort,Using tempoary |

Count()和Max()的优化

```
//查询最后支付时间--优化max()函数
explain select max(payment_date) from payment;
create index idx_paydate on payment(payment_data);//给payment_date建立索引(覆盖索引)

//在一条SQL中同时查出2006年和2007年电影的数量--优化Count()函数
select count(release_year='2006' or null) as '2006年电影数量'，count(release_year='2007' or null) as '2007年电影数量' from film;
//有关count()函数
https://blog.csdn.net/wendychiang1991/article/details/70909958/
```

子查询优化

```
通常情况下，需要把子查询优化为join查询，但在优化时要注意关联键是否有一对多的关系，要注意重复数据。(distinct去重)
//查询sandra出演的所有影片
explain select title,release_year,LENGTH from film
where film_id in (
select film_id from film_actor where actor_id in (
select actor_id from actor where first_name='sandra'));
```

group by的优化

```
//改前 临时表
explain select actor.first_name,actor_last_name,count(*) from sakila.film_actor
inner join sakila.actor USING(actor_id)
group by film_actor.actor_id;
//改后 结合子查询 索引
explain select actor.first_name,actor.last_name,c.cnt from sakila.film_actor
inner join (
select actor_id,count(*) as cnt from sakila.film_actor group by actor_id) as c USING(actor_id);
```

limit优化

```
limit常用于分页处理，时常会伴随order by 从句使用，因此大多时候会使用Filesorts这样会造成大量的IO问题。

//文件排序，IO大
explain select film_id,description from sakila.film order by title limit 50,5;
1.优化：使用有索引的列或主键进行order by操作（order by film_id）
2.记录上次返回的主键，在下次查询的时候用主键过滤，避免了数据量大时扫描过多的记录
select film_id,description from sakila.film where film_if>55 and film_id<=60 order by film_id limit 1,5; 
页数越大，IO越大
```

#### 索引优化

如何选择合适的列建立索引？

```
1.在where从句，group by从句，order by从句，on从句中出现的列(select)
2.索引字段越小越好(表每页数据才会更多，IO效率会更高)
3.离散度大的列放到联合索引的前面
select * from payment where staff_id=2 and customer_id=584;
index(staff_id,customer_id)好？还是index(customer_id,staff_id)好？
由于customer_id的离散度更大(重复率小,可选择性更大)，所以应该使用index(customer_id,staff_id)
```

索引优化SQL的方法

索引的维护及优化--重复及冗余索引

```
冗余索引是指多个索引的前缀列相同，或是在联合索引中包含了主键的索引。如下：key(name,id)就是一个冗余索引
create table test(
id int not null primary key,
name varchar(10) not null,
key(name,id)
)engine=innodb;
//可以删除冗余索引，达到优化效果。

使用pt-duplicate-key-checker工具检查重复及冗余索引
pt-duplicate-key-checker \
-uroot \
-p '' \
-h 127.0.0.1
```

索引维护的方法--删除不用索引

```
目前mysql中还没有记录索引的使用情况，但是在PerconMySQL和MariaDB中可通过INDEX_STATISTICS表来查看哪些索引未使用，但在mysql中目前只能通过慢查日志配合pt-index-usage工具来进行索引使用情况分析。
pt-index-usage \
	-uroot -p'' \
	mysql-slow.log
```

#### 数据库表结构优化

1、选择**合适**的数据类型

```
1.使用可以存下你的数据的最小的数据类型
2.使用简单的数据类型。int要比varchar类型在mysql处理上更简单
3.尽可能的使用not null定义字段
4.尽量少用text类型，非用不可时最好考虑分表
*使用int来存储日志时间，利用FROM_UNIXTINE()(得到日期),UNIX_TIMESTAMP()(得到时间戳)两个函数来进行转换
*使用bigint来存ip地址，利用INET_ATON(),INET_NTOA()两个函数来进行转换
```

2、表的范式化和反范式化

**范式化**是指数据库设计的规范，目前说到范式化一般是指第三设计范式，也就是要求数据表中不存在非关键字段对任意候选关键字段的传递函数依赖则符合第三范式。

```
不符合第三范式要求的表存在下列问题：
1.数据冗余：（分类，分类描述）对于每一个商品都会进行记录
2.数据的插入异常
3.数据的更新异常
4.数据的删除异常
```

**反范式化**是指为了查询效率的考虑把原本符合第三范式的表适当的增加冗余，以达到优化查询的目的，反范式化是一种以空间来换取时间的操作。

3、表的拆分

垂直拆分

```
所谓的垂直拆分，就是把原来一个有很多列的表拆分成多个表，这解决了表的宽度问题。通常垂直拆分可以按以下原则进行：
1.把不常用的字段单独存放到一个表中
2.把大字段独立存放到一个表中
3.把经常一起使用的字段放到一起
```

水平拆分

```
表的水平拆分是为了解决单表的数据量过大的问题，水平拆分的表每一个表的结构都是完全一致的。
常用的水平拆分方法为：
1.对id进行hash运算，如果要拆分成5个表则使用mod(id,5)去除0-4个值
2.针对不同的hashID把数据存到不同的表中
```



#### 系统配置优化

1、操作系统配置优化

数据库是基于操作系统的，目前大多数mysql都是安装在Linux系统之上，所以对于操作系统的一些参数配置也会影响到MYSQL的性能。

```
网络方面的配置，要修改/etc/stysctl.conf文件
#增加tcp支持的队列数
net.ipv4.tcp_max_syn_backlog = 65535
#减少断开链接是，资源回收
net.ipv4.tcp_max_tw_buckets = 8000
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_tw_recycle = 1
net.ipv4.tcp_fin_timeout = 10

打开文件数的限制，可以使用ulimit -a 查看目录的各位限制，可以修改/etcsecurity/limitsconf文件，增加一下内容以修改打开文件数量的限制
*soft nofile 65535
*hard nofile 65535
除此之外最好在mysql服务器上关闭iptables，selinux等防火墙软件。
```

2、MySQL数据库优化

MySQL配置文件

```
mysql可以通过启动时指定配置参数和使用配置文件两种方法进行配置，在大数情况下配置文件位于/etc/my.cnf或是/etc/mysql/my.cnf在windows系统配置文件可以是位于C:/windows/my.ini文件，mysql查找配置文件的顺序可以通过一下方法获得
/usr/sbin/mysqld --verbose --help | grep -A 1 ' Default options '
```

MySQL配置文件--常用参数说明

```
1.innodb_buffer_pool_size >= total MB
非常重要的一个参数，用于配置innodb的缓冲池，如果数据库中只有innodb表，则推荐配置量为总内存的75%

2.innodb_buffer_pool__instances
MySQL5.5中新增加参数，可以控制缓冲池的个数，默认情况下只有一个缓冲池。

3.innodb_log_buffer_size
innodb log缓冲的大小，由于日志最长每秒钟就会刷新所以一般不用太大。

4.innodb_flush_log_at_trx_commit
关键参数，对innodb的IO效率影响很大。默认值为1，可取0，1，2三个值，一般建议为2，但如果数据安全性要求比较高则使用默认值1.

5.innodb_read_io_threads   innodb_write_io_threads
以上两个参数决定了Innodb读写的IO进程数，默认为4.

6.innodb_file_per_table
关键参数，控制innodb每一个表使用独立的表空间，默认为off，也就是所有表都会建立在共享表空间中。

7.innodb_stats_on_metadata
决定了mysql在什么情况下会刷新innodb表的统计信息。
```

3、第三方配置工具

链接地址：https://tools.percona.com/wizard

#### 服务器硬件优化

如何选择cpu？

```
1.mysql有一些工作只能使用到单核cpu，Replicate,SQL...
2.mysql对cpu核数的支持并不是越多越快。mysql5.5使用的服务器不要超过30核
```

磁盘IO优化

```
常用RAID级别简介
RAID0：也称条带，就是把多个磁盘链接成一个硬盘使用，这个级别IO最好
RAID1：也称镜像，要求至少有两个磁盘，每组磁盘存储的数据相同
RAID5：也是把多个硬盘合并成一个逻辑盘使用，数据读写时会建立奇偶校验信息，分别存储在不同磁盘上。
RAID1+0：就是RAID1和RAID0的结合。同时具备两个级别的优缺点。一般建议数据库使用这个级别。

SNA和NAT是否适合数据库？
1.常用于高可用解决方案
2.顺序读写效率很高，但是随机读写不如人意
3.数据库随机读写比率很高
```
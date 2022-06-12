---
title: "MyBatis-Plus入门1"
date: 2016-06-17 21:22:30
draft: true
tags: [Java, MyBatis]
categories:
- [Java, MyBatis]
---

#### 课程资料
视频地址：https://www.imooc.com/learn/1130
文档地址：https://mp.baomidou.com/guide/

#### pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.3.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <groupId>com.mp</groupId>
    <artifactId>first</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <!-- spring boot 启动器-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>

        <!-- spring boot test 启动器-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

        <!-- lombok简化java代码 如果没有安装，先安装这个插件-->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>

        <!-- mybatis-plus插件 -->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.1.0</version>
        </dependency>

        <!-- mysql jdbc驱动 -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
    </dependencies>

</project>
```

#### application.yml

```yaml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/mp?useSSL=false&serverTimezone=GMT%2B8
    username: root
    password: root

logging:
  level:
    root: warn
    com.mp.dao: trace
  pattern:
    console: '%p%m%n'
    # %p: 日志级别
    # %m: 日志内容
    # %n: 换行
```
#### 常用注解
| 注解                     | 作用                                                         |
| ------------------------ | ------------------------------------------------------------ |
| @Data                    | 注解是lombok.jar包下的注解，该注解通常用在实体bean上，不需要写出set和get方法，但是具备实体bean所具备的方法，简化编程提高变成速度 |
| @TableId                 | 设置主键，mp默认匹配数据表中的id字段为主键，如果没有id字段，需要在实体类使用该注解表名主键字段 |
| @TableName("mp_user")    | 设置表名，表名和实体类的名字不一致                           |
| @TableField("real_name") | 设置字段名，表中字段名和实体类的名字不一致                   |

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019122500464781.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)
 ##### 当一个字段在表中忽略 :
 1. 用transient 不可序列化  
 `private transient String remark;`
 2. 用static 许自动生成setget 
 `private static String remark;`
 3. @TableField(exist=false)

##### 普通查询
1. selectById
2. selectBatchIds，实际查询是 select  where id in (id1, id2, id3)
3. selectByMap, 实际是拼接where条件查询，其中**map里面的key和数据表字段大小写一致**


```java
package com.mp;

import com.mp.dao.UserMapper;
import com.mp.entity.User;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

import java.util.Arrays;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

/**
 * @Description
 * @auther mohuani
 * @create 2019-12-25 11:37
 */
@RunWith(SpringRunner.class)
@SpringBootTest
public class RetrieveTest {

    @Autowired
    private UserMapper userMapper;

    @Test
    public void selectById() {
        User user = userMapper.selectById(1088250446457389058L);
        System.out.println(user);
    }

    @Test
    public void selectBatchIds() {
        List<Long> list = Arrays.asList(1088248166370832385L, 1094590409767661570L, 1209509417456001025L);
        List<User> userList = userMapper.selectBatchIds(list);
        userList.forEach(System.out::println);
    }

    @Test
    public void selectByMap() {
        Map<String, Object> columnMap = new HashMap<>();
        columnMap.put("name", "李艺伟");
        columnMap.put("age", 28);
        List<User> userList = userMapper.selectByMap(columnMap);
        userList.forEach(System.out::println);
    }
}

```
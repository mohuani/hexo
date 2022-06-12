---
title: "MyBatis-Plus分页无效"
date: 2022-04-28 16:22:30
draft: true
tags: [Java, MyBatis-Plus]
categories:
- [Java, MyBatis-Plus]
---

### 问题
最新刚开始使用Springboot + Mybatis-Plus 写Java的项目，curd进行分页查询的时候，发现生成的sql并没有加上 `limit` ，也就是说分页功能是失效的。


MyBatis-Plus的官方文档：https://baomidou.com/pages/2976a3/#spring

我使用的 MyBatis-Plus 版本是

```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.5.1</version>
</dependency>
```

### 解决方法

#### 1、添加 MybatisPlusConfig

```java
package com.xxx;

import com.baomidou.mybatisplus.annotation.DbType;
import com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor;
import com.baomidou.mybatisplus.extension.plugins.inner.PaginationInnerInterceptor;
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
@MapperScan(basePackages = "com.xxx.dao")
public class MyBatisPlusConfig {

    @Bean
    public MybatisPlusInterceptor innerInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.MYSQL));
        return interceptor;
    }
}

```

#### 2、将 MybatisPlusInterceptor 注册到 DataSourceConfig中的 SqlSessionFactory里面去 

```java

package com.xxx;

import com.baomidou.mybatisplus.core.MybatisConfiguration;
import com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor;
import com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean;
import org.apache.ibatis.plugin.Interceptor;
import org.apache.ibatis.session.SqlSessionFactory;
import org.mybatis.spring.SqlSessionTemplate;
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.boot.jdbc.DataSourceBuilder;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Primary;
import org.springframework.core.io.support.PathMatchingResourcePatternResolver;
import org.springframework.core.io.support.ResourcePatternResolver;
import org.springframework.jdbc.datasource.DataSourceTransactionManager;

import javax.sql.DataSource;

@Configuration
@MapperScan(basePackages = {"com.xxx.dao"}, sqlSessionTemplateRef = "SqlSessionTemplate")
public class xxxDSConfig {


    @Autowired
    public MybatisPlusInterceptor mybatisPlusInterceptor;

    @Primary
    @Bean(name = "xxxSqlSessionFactory")
    public SqlSessionFactory xxxSqlSessionFactory(@Qualifier("xxxDataSource") DataSource dataSource,
                                                     @Qualifier("xxxMybatisConfig") MybatisConfiguration config) {
        MybatisSqlSessionFactoryBean mybatisSqlSessionFactoryBean = new MybatisSqlSessionFactoryBean();
        mybatisSqlSessionFactoryBean.setDataSource(dataSource);
        mybatisSqlSessionFactoryBean.setConfiguration(config);

        Interceptor[] plugins = {mybatisPlusInterceptor};
        mybatisSqlSessionFactoryBean.setPlugins(plugins);

        try {
            ResourcePatternResolver resolver = new PathMatchingResourcePatternResolver();
            mybatisSqlSessionFactoryBean.setMapperLocations(resolver.getResources("classpath:/mapper/*Mapper.xml"));
            return mybatisSqlSessionFactoryBean.getObject();
        } catch (Exception e) {
            e.printStackTrace();
            throw new RuntimeException(e);
        }
    }

}







```













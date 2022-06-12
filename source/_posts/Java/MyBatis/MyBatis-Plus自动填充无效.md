---
title: "MyBatis-Plus自动填充无效"
date: 2022-05-09 16:22:30
draft: true
tags: [Java, MyBatis-Plus]
categories:
- [Java, MyBatis-Plus]
---

### 问题
> 最新刚开始使用Springboot + Mybatis-Plus 写Java的项目，发现created_at，updated_at这种与业务无关的时间海妖自己去管理，实在是不方便，不如像Laravel那样在框架层直接自动维护。Mybatis-Plus官方文档给出来的demo比较简单，本人照着葫芦画瓢没有成功，后面经过查询各种博客解决了这个问题。


MyBatis-Plus的官方文档：https://baomidou.com/pages/2976a3/#spring

我使用的 MyBatis-Plus 版本是

```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.5.1</version>
</dependency>
```


### 通常原因

- 1.先检查字段属性是否一致问题
- 2.检查时间类型问题,使用 java.util下的 `Date`类，还是使用的 `LocalDateTime` 类
- 3.检查fill 有没有指派正确

```java
@TableField(value = “created_at”, fill = FieldFill.INSERT)
@TableField(value = “updated_at”, fill =FieldFill.INSERT_UPDATE)
```

- 4.检查 `MyBatisPlusMetaObjectHandler` 是否注册到 `GlobalConfig`
- 5.检查 `GlobalConfig` 是否注册到 `MybatisSqlSessionFactoryBean`


### 解决方法

#### 1、添加 MyBatisPlusMetaObjectHandler

- 其中要自动维护的字段和你项目中 Model 文件的字段大小写保持一致就行了，不是和数据表中的字段大小写保持一致，我已经测试过了
- `updateFill()` 时 `strictUpdateFill()` 如果使用  `this.strictUpdateFill(metaObject, "updatedAt", Date.class, new Date())` 方式，会发现程序确实被拦截走到了自动`updateFill()`这里，但是没有填充成功，后面尝试使用 `setFieldValByName`来进行自动填充是成功的，这里应该是个bug，也有可以能是我操作姿势不对。

```java
package com.xxx;
package com.mainto.earthen.retail.service.datasource;

import com.baomidou.mybatisplus.core.handlers.MetaObjectHandler;
import org.apache.ibatis.reflection.MetaObject;
import org.springframework.stereotype.Component;

import java.util.Date;

@Component
public class MyBatisPlusMetaObjectHandler implements MetaObjectHandler {
    @Override
    public void insertFill(MetaObject metaObject) {
        this.strictInsertFill(metaObject, "createdAt", Date.class, new Date()); // 起始版本 3.3.3(推荐)
        this.strictInsertFill(metaObject, "updatedAt", Date.class, new Date()); // 起始版本 3.3.3(推荐)
    }

    @Override
    public void updateFill(MetaObject metaObject) {
        // this.strictUpdateFill(metaObject, "updatedAt", Date.class, new Date()); // 起始版本 3.3.3(推荐) // 这种方式写bug，没有效果
        this.setFieldValByName("updatedAt", new Date(), metaObject);
    }
}


```

#### 2、将 MybatisPlusInterceptor 注册到 DataSourceConfig中的 SqlSessionFactory里面去 

只需要添加下面的3行代码
```java
// MetaObjectHandler 配置
GlobalConfig globalConfig = new GlobalConfig();
globalConfig.setMetaObjectHandler(new MyBatisPlusMetaObjectHandler());
mybatisSqlSessionFactoryBean.setGlobalConfig(globalConfig);

```


```java

package com.xxx;

import com.baomidou.mybatisplus.core.MybatisConfiguration;
import com.baomidou.mybatisplus.core.config.GlobalConfig;
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
        // 数据库配置
        MybatisSqlSessionFactoryBean mybatisSqlSessionFactoryBean = new MybatisSqlSessionFactoryBean();
        mybatisSqlSessionFactoryBean.setDataSource(dataSource);
        mybatisSqlSessionFactoryBean.setConfiguration(config);

        // 分页器配置 MybatisPlusInterceptor
        Interceptor[] plugins = {mybatisPlusInterceptor};
        mybatisSqlSessionFactoryBean.setPlugins(plugins);

        // MetaObjectHandler 配置
        GlobalConfig globalConfig = new GlobalConfig();
        globalConfig.setMetaObjectHandler(new MyBatisPlusMetaObjectHandler());
        mybatisSqlSessionFactoryBean.setGlobalConfig(globalConfig);

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

参考文章： https://blog.csdn.net/m0_55413703/article/details/121944990


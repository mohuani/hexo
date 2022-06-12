---
title: "MyBatis-Plus入门2"
date: 2016-06-17 21:22:30
draft: true
tags: [Java, MyBatis]
categories:
- [Java, MyBatis]
---

#### 查询方法

```java
package com.mp;

import com.baomidou.mybatisplus.core.conditions.Wrapper;
import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
import com.baomidou.mybatisplus.core.toolkit.Wrappers;
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

    /**
     * 1、名字中包含雨并且年龄小于40
     * 	name like '%雨%' and age<40
     */
    @Test
    public void selectByWrapper() {
        QueryWrapper<User> queryWrapper = new QueryWrapper<>();
        queryWrapper.like("name", "雨").lt("age", 40);

        List<User> userList = userMapper.selectList(queryWrapper);
        userList.forEach(System.out::println);
    }

    /**
     * 2、名字中包含雨年并且龄大于等于20且小于等于40并且email不为空
     *    name like '%雨%' and age between 20 and 40 and email is not null
     */
    @Test
    public void selectByWrapper2() {
        QueryWrapper<User> queryWrapper = new QueryWrapper<>();
        queryWrapper.like("name", "雨").between("age" ,20 ,40).isNotNull("email");

        List<User> userList = userMapper.selectList(queryWrapper);
        userList.forEach(System.out::println);
    }

    /**
     * 3、名字为王姓或者年龄大于等于25，按照年龄降序排列，年龄相同按照id升序排列
     *    name like '王%' or age>=25 order by age desc,id asc
     */
    @Test
    public void selectByWrapper3() {
        QueryWrapper<User> queryWrapper = new QueryWrapper<>();
        queryWrapper.likeRight("name", "王").or().gt("age", 25).orderByDesc("age").orderByAsc("id");

        List<User> userList = userMapper.selectList(queryWrapper);
        userList.forEach(System.out::println);
    }

    /**
     * 4、创建日期为2019年2月14日并且直属上级为名字为王姓
     *       date_format(create_time,'%Y-%m-%d')='2019-02-14' and manager_id in (select id from user where name like '王%')
     */
    @Test
    public void selectByWrapper4() {
        QueryWrapper<User> queryWrapper = new QueryWrapper<>();
        queryWrapper.apply("date_format(create_time, '%Y-%m-%d') = {0}", "2019-02-14")
                .inSql("manager_id", "select id from user where name like '王%'");

        List<User> userList = userMapper.selectList(queryWrapper);
        userList.forEach(System.out::println);
    }

    /**
     * 5、名字为王姓并且（年龄小于40或邮箱不为空）
     *     name like '王%' and (age<40 or email is not null)
     */
    @Test
    public void selectByWrapper5() {
        QueryWrapper<User> queryWrapper = new QueryWrapper<>();
        queryWrapper.likeRight("name", "王").and(userQueryWrapper -> userQueryWrapper.gt("age", 40).or().isNotNull("email"));

        List<User> userList = userMapper.selectList(queryWrapper);
        userList.forEach(System.out::println);
    }

    /**
     * 6、名字为王姓或者（年龄小于40并且年龄大于20并且邮箱不为空）
     *     name like '王%' or (age<40 and age>20 and email is not null)
     */
    @Test
    public void selectByWrapper6() {
        QueryWrapper<User> queryWrapper = new QueryWrapper<>();
        queryWrapper.likeRight("name", "王").or(userQueryWrapper -> userQueryWrapper.lt("age", 40).gt("age", 20).isNotNull("email"));
        List<User> userList = userMapper.selectList(queryWrapper);
        userList.forEach(System.out::println);
    }

    /**
     * 7、（年龄小于40或邮箱不为空）并且名字为王姓
     *     (age<40 or email is not null) and name like '王%'
     */
    @Test
    public void selectByWrapper7() {
        QueryWrapper<User> queryWrapper = new QueryWrapper<>();
        queryWrapper.nested(userQueryWrapper -> userQueryWrapper.lt("age", 40).or().isNotNull("email"))
                .likeRight("name", "王");
        List<User> userList = userMapper.selectList(queryWrapper);
        userList.forEach(System.out::println);
    }

    /**
     * 8、年龄为30、31、34、35
     *     age in (30、31、34、35)
     */
    @Test
    public void selectByWrapper8() {
        QueryWrapper<User> queryWrapper = new QueryWrapper<>();
        queryWrapper.in("age", Arrays.asList(30, 31, 34, 35));
        List<User> userList = userMapper.selectList(queryWrapper);
        userList.forEach(System.out::println);
    }

    /**
     * 9、只返回满足条件的其中一条语句即可
     * limit 1
     */
    @Test
    public void selectByWrapper9() {
        QueryWrapper<User> queryWrapper = new QueryWrapper<>();
        queryWrapper.in("age", Arrays.asList(30, 31, 34, 35)).last("limit 1");
        List<User> userList = userMapper.selectList(queryWrapper);
        userList.forEach(System.out::println);
    }


    /**
     * 二、select中字段不全部出现的查询
     * 10.1、名字中包含雨并且年龄小于40(需求1加强版)
     *     第一种情况：select id,name
     *     from user
     *     where name like '%雨%' and age<40
     */
    @Test
    public void selectByWrapperSupper() {
        QueryWrapper<User> queryWrapper = new QueryWrapper<>();
        queryWrapper.select("id, name").like("name", "雨").lt("age", 40);
        List<User> userList = userMapper.selectList(queryWrapper);
        userList.forEach(System.out::println);
    }


    /**
     * 10.2第二种情况：select id,name,age,email
     *     from user
     *     where name like '%雨%' and age<40
     */
    @Test
    public void selectByWrapperSupper2() {
        QueryWrapper<User> queryWrapper = new QueryWrapper<>();
        queryWrapper.like("name", "雨").lt("age", 40)
                .select(User.class, tableFieldInfo -> !tableFieldInfo.getColumn().equals("create_time") &&
                        !tableFieldInfo.getColumn().equals("manager_id"));
        List<User> userList = userMapper.selectList(queryWrapper);
        userList.forEach(System.out::println);
    }

	@Test
    public void selectByWrapperMaps() {
        QueryWrapper<User> queryWrapper = new QueryWrapper<>();
        queryWrapper.select("id", "name").like("name", "雨").lt("age", 40);

        List<Map<String, Object>> userList = userMapper.selectMaps(queryWrapper);
        userList.forEach(System.out::println);
    }

    /**
     * 11、按照直属上级分组，查询每组的平均年龄、最大年龄、最小年龄。
     * 并且只取年龄总和小于500的组。
     * select avg(age) avg_age,min(age) min_age,max(age) max_age
     * from user
     * group by manager_id
     * having sum(age) <500
     */
    @Test
    public void selectByWrapperMaps2() {
        QueryWrapper<User> queryWrapper = new QueryWrapper<>();
        queryWrapper.select("avg(age) avg_age", "min(age) min_age", "max(age) max_age")
                .groupBy("manager_id").having("sum(age)<{0}", 500);

        List<Map<String, Object>> userList = userMapper.selectMaps(queryWrapper);
        userList.forEach(System.out::println);
    }
    
}

```

**实体作为条件构造器构造方法的参数时**
- 实体参数和条件构造器构造的条件互不干扰，都会拼接到SQL语句里面，所以用的时候选择其中的一种方式就行了。

**allEq**

```java
allEq(Map<R, V> params)
allEq(Map<R, V> params, boolean null2IsNull)
allEq(boolean condition, Map<R, V> params, boolean null2IsNull)
```
- 全部eq(或个别isNull)

> params : key为数据库字段名,value为字段值
> null2IsNull : 为true则在map的value为null时调用 isNull 方法,为false时则忽略value为null的

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191225180435655.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=,size_16,color_FFFFFF,t_70)

参考： https://mp.baomidou.com/guide/wrapper.html#alleq

**selectObjs**
- 只会返回结果集的第一列，其他列不返回，比较鸡肋


**selectCount**
- 查询总记录数

**selectOne**
- 返回一个实体对象，如果查询的结果不止一条就会直接报错


#### Lambda查询
- 编写查询条件的时候我们容易把字段名写错，但是编译检查不出来。使用Lambda的方式，可以减少字段名写错的情况，在编译的时候会直接检测出来。

```java
	// 第一种构建方式
	LambdaQueryWrapper<User> lambda = new QueryWrapper<User>().lambda();
	// 第二种构建方式
	LambdaQueryWrapper<User> userLambdaQueryWrapper = new LambdaQueryWrapper<>();
	// 第三种构建方式
	LambdaQueryWrapper<User> lambdaQuery = Wrappers.<User>lambdaQuery();
```
查询示例代码
```java
	@Test
    public void selectLambda() {
        // LambdaQueryWrapper<User> lambda = new QueryWrapper<User>().lambda();
        // LambdaQueryWrapper<User> userLambdaQueryWrapper = new LambdaQueryWrapper<>();
        LambdaQueryWrapper<User> lambdaQuery = Wrappers.<User>lambdaQuery();
        lambdaQuery.like(User::getName, "雨").lt(User::getAge, 40);

        List<User> userList = userMapper.selectList(lambdaQuery);
        userList.forEach(System.out::println);
    }

    /**
     *  5.名字为王姓并且（年龄小于40或邮箱不为空）
     *     name like '王%' and (age<40 or email is not null)
     */
    @Test
    public void selectLambda2() {
        LambdaQueryWrapper<User> lambdaQuery = Wrappers.lambdaQuery();
        lambdaQuery.likeRight(User::getName, "王")
                .and(userLambdaQueryWrapper -> userLambdaQueryWrapper.lt(User::getAge, 40).or().isNotNull(User::getEmail));
        List<User> userList = userMapper.selectList(lambdaQuery);
        userList.forEach(System.out::println);
    }

    @Test
    public void selectLambda3() {
        List<User> userList = new LambdaQueryChainWrapper<User>(userMapper)
                .like(User::getName, "雨").ge(User::getAge, 20).list();

        userList.forEach(System.out::println);
    }
```

#### 自定义查询

```java
package com.mp.dao;

import com.baomidou.mybatisplus.core.conditions.Wrapper;
import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.baomidou.mybatisplus.core.toolkit.Constants;
import com.mp.entity.User;
import org.apache.ibatis.annotations.Param;
import org.apache.ibatis.annotations.Select;

import java.util.List;

/**
 * @Description
 * @auther mohuani
 * @create 2019-12-24 22:55
 */

public interface UserMapper extends BaseMapper<User> {

	// 此处的ew，写法是固定的，框架约定的名字
    @Select("select * from user ${ew.customSqlSegment}")
    List<User> selectAll(@Param(Constants.WRAPPER) Wrapper<User> wrapper);
}

```

```java
	@Test
    public void selectMy() {
        LambdaQueryWrapper<User> lambdaQuery = Wrappers.lambdaQuery();
        lambdaQuery.likeRight(User::getName, "王")
                .and(userLambdaQueryWrapper -> userLambdaQueryWrapper.lt(User::getAge, 40).or().isNotNull(User::getEmail));
        List<User> userList = userMapper.selectAll(lambdaQuery);
        userList.forEach(System.out::println);
    }
```
也可以通过xml配置自定义查询

#### 分页查询

```java
// 无条件翻页查询
IPage<T> page(IPage<T> page);
// 翻页查询
IPage<T> page(IPage<T> page, Wrapper<T> queryWrapper);
// 无条件翻页查询
IPage<Map<String, Object>> pageMaps(IPage<T> page);
// 翻页查询
IPage<Map<String, Object>> pageMaps(IPage<T> page, Wrapper<T> queryWrapper);
```

```shell
分页查询步骤:
1:创建并完善配置类MybatisPlusConfig.java
2.实例化Page对象
 2.1: Page对象构造函数参数:
   1:当前页
   2:一页的数量
   3.分页总数
   4:是否需要查询总条数(false:不查,true:查,少发出一条sql)
3.1 使用selectPage 或 selectMapsPage(区别:前者封装进实体类中,后者封装进Map对象中)
3.2 如果为多表查询,则需要进行自定义方法,此时需要配置UserMapper接口文件,返回值为IPage类型
    注:切记不可返回Page类型,否者代码运行无报错,也能看到sql查询,但是在获取getRecords时无数据
   3.2.1 IPage<User> selectAllByPage(Page<User> page, @Param(Constants.WRAPPER) Wrapper<User> wrapper);
   3.2.2 配置@select注解 或者 配置xml文件
      @select注解附:@Select("select * from User ${ew.customSqlSegment}")
      xml配置附:<select id="selectAllByPageXml" resultType="com.mp.pojo.User">
              select * from User ${ew.customSqlSegment}
           </select>
4.传入参数Page对象和QueryWrapper对象
   4.1: 使用getTotal获取总条数
   4.2: 使用getPages获取总页数
```

参考：https://mp.baomidou.com/guide/crud-interface.html#page
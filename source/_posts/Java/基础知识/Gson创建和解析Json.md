---
title: "Gson创建和解析Json"
date: 2016-06-15 21:22:30
draft: true
tags: [Java]
categories:
- [Java]
---

课程地址：https://www.imooc.com/learn/523
Gson是google开发的一个开源Json解析库，最新版本以及源码可以在官方的github上查看：https://github.com/google/gson

##### Maven依赖

```maven
<dependency>
  <groupId>com.google.code.gson</groupId>
  <artifactId>gson</artifactId>
  <version>2.8.5</version>
</dependency>
```
##### 创建javaBean

```java
package bean;

import com.google.gson.annotations.SerializedName;

import java.util.Arrays;

/**
 * @Description
 * @auther mohuani
 * @create 2019-05-08 23:00
 */
public class Diaosi {

//    @SerializedName("NAME")  // 通过注解将属性name变成NAME
    private String name;
    private String school;
    private boolean has_girlfriend;
    private double age;
    private Object car;
    private Object house;
    private String[] major;
    private String comment;
    private String birthday;


	// 如果用transient声明一个实例变量，当对象存储时，它的值不需要维持。换句话来说就是，用transient关键字标记的成员变量不参与序列化过程
    private transient String ignore;

    public String getIgnore() {
        return ignore;
    }

    public void setIgnore(String ignore) {
        this.ignore = ignore;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getSchool() {
        return school;
    }

    public void setSchool(String school) {
        this.school = school;
    }

    public boolean isHas_girlfriend() {
        return has_girlfriend;
    }

    public void setHas_girlfriend(boolean has_girlfriend) {
        this.has_girlfriend = has_girlfriend;
    }

    public double getAge() {
        return age;
    }

    public void setAge(double age) {
        this.age = age;
    }

    public Object getCar() {
        return car;
    }

    public void setCar(Object car) {
        this.car = car;
    }

    public Object getHouse() {
        return house;
    }

    public void setHouse(Object house) {
        this.house = house;
    }

    public String[] getMajor() {
        return major;
    }

    public void setMajor(String[] major) {
        this.major = major;
    }

    public String getComment() {
        return comment;
    }

    public void setComment(String comment) {
        this.comment = comment;
    }

    public String getBirthday() {
        return birthday;
    }

    public void setBirthday(String birthday) {
        this.birthday = birthday;
    }

    @Override
    public String toString() {
        return "Diaosi{" +
                "name='" + name + '\'' +
                ", school='" + school + '\'' +
                ", has_girlfriend=" + has_girlfriend +
                ", age=" + age +
                ", car=" + car +
                ", house=" + house +
                ", major=" + Arrays.toString(major) +
                ", comment='" + comment + '\'' +
                ", birthday='" + birthday + '\'' +
                '}';
    }
}

```

ps:关键字  **transient**  如果用transient声明一个实例变量，当对象存储时，它的值不需要维持。换句话来说就是，用transient关键字标记的成员变量不参与序列化过程。
##### 生成json

```java
package gson;

import bean.Diaosi;
import com.google.gson.FieldNamingStrategy;
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;

import java.lang.reflect.Field;

/**
 * @Description
 * @auther mohuani
 * @create 2019-05-09 0:15
 */
public class GsonCreateSample {
    public static void main(String[] args) {
        Diaosi diaosi = new Diaosi();
        diaosi.setName("wfk");
        diaosi.setAge(25.2);
        diaosi.setBirthday("1996-07-01");
        diaosi.setSchool("蓝翔");
        diaosi.setMajor(new String[] {"理发", "挖掘机"});
        diaosi.setHas_girlfriend(true);
        diaosi.setCar(null);
        diaosi.setHouse(null);
        diaosi.setComment("这是一个注释1");
        diaosi.setIgnore("忽略这条属性");

        Gson gson = new Gson();
        System.out.println(gson.toJson(diaosi));

        // 美化调试
        GsonBuilder gsonBuilder = new GsonBuilder();
        gsonBuilder.setPrettyPrinting();
        gsonBuilder.setFieldNamingStrategy(new FieldNamingStrategy() {
            @Override
            public String translateName(Field field) {
                // 将原有的属性name变成NAME
                if (field.getName().equals("name")) {
                    return "NAME";
                }
                return field.getName();
            }
        });
        Gson gson1 = gsonBuilder.create();
        System.out.println(gson1.toJson(diaosi));
    }
}

```
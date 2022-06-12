---
title: "MapStruct"
date: 2022-05-13 14:22:30
draft: true
tags: [Java, MapStruct]
categories:
- [Java, MapStruct]
---

### 官方文档：
https://mapstruct.org/



### 场景介绍
现在有这么个场景，从数据库查询出来了一个user对象（包含id，用户名，密码，手机号，邮箱，角色这些字段）和一个对应的角色对象role（包含id，角色名，角色描述这些字段），现在在controller需要用到user对象的id，用户名，和角色对象的角色名三个属性。一种方式是直接把两个对象传递到controller层，但是这样会多出很多没用的属性。更通用的方式是需要用到的属性封装成一个类(DTO)，通过传输这个类的实例来完成数据传输。这时候我们需要对Java中的两个Bean进行转换，MapStruct为了提供了方便快捷的操作，不仅可以 ObjectA 转 ObjectB，还可以 List<ObjectA> 转 List<ObjectB>。
  
  
### 官网示例

#### 一、两个需要被互相转换的对象
假设我们有一个表示汽车的类（例如JPA实体）和一个随附的数据传输对象（DTO）。这两种类型非常相似，只是席位计数属性具有不同的名称，并且 type 属性在类中属于特殊枚举类型，但在 DTO 中是纯字符串。Car
  
- Car.java
```java
public class CarDto {

    private String make;
    private int seatCount;
    private String type;

    //constructor, getters, setters etc.
}
```
- CarDto.java
```java
public class CarDto {
 
    private String make;
    private int seatCount;
    private String type;

    //constructor, getters, setters etc.
}
```
#### 映射器接口
要生成用于从对象创建对象的映射器，需要定义映射器接口：CarDto Car
	
	
- CarMapper.java	
```java
@Mapper
public interface CarMapper {

    CarMapper INSTANCE = Mappers.getMapper( CarMapper.class );

    @Mapping(source = "numberOfSeats", target = "seatCount")
    CarDto carToCarDto(Car car);
}
```
- 注释@Mapper1将接口标记为映射接口，并允许 MapStruct 处理器在编译期间启动。实际映射方法2期望源对象作为参数并返回目标对象。它的名字可以自由选择。

	
- 对于源对象和目标对象中具有不同名称的属性，可以使用注释来配置名称。@Mapping在必要且可能的情况下，将为源和目标中具有不同类型的属性执行类型转换，例如，属性将从枚举类型转换为字符串。type
	
	
- 当然，一个接口中可以有多个映射方法，所有这些方法的实现都将由MapStruct生成。可以从类中检索接口实现的实例。按照约定，接口声明成员MappersINSTANCE，为客户端提供对映射器实现的访问。
  
  
  
#### 使用映射器
基于映射器接口，客户端可以以非常简单且类型安全的方式执行对象映射：
	
- CarMapperTest.java
```java
@Test
public void shouldMapCarToDto() {
    //given
    Car car = new Car( "Morris", 5, CarType.SEDAN );

    //when
    CarDto carDto = CarMapper.INSTANCE.carToCarDto( car );

    //then
    assertThat( carDto ).isNotNull();
    assertThat( carDto.getMake() ).isEqualTo( "Morris" );
    assertThat( carDto.getSeatCount() ).isEqualTo( 5 );
    assertThat( carDto.getType() ).isEqualTo( "SEDAN" );
}
```
	
### SpringBoot集成MapStruct

    
#### 安装 
Maven 引入对应的dependency 

```xml
...
<properties>
    <org.mapstruct.version>1.4.2.Final</org.mapstruct.version>
</properties>
...
<dependencies>
    <dependency>
        <groupId>org.mapstruct</groupId>
        <artifactId>mapstruct</artifactId>
        <version>${org.mapstruct.version}</version>
    </dependency>
</dependencies>
...
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.8.1</version>
            <configuration>
                <source>1.8</source> <!-- depending on your project -->
                <target>1.8</target> <!-- depending on your project -->
                <annotationProcessorPaths>
                    <path>
                        <groupId>org.mapstruct</groupId>
                        <artifactId>mapstruct-processor</artifactId>
                        <version>${org.mapstruct.version}</version>
                    </path>
                    <!-- other annotation processors -->
                </annotationProcessorPaths>
            </configuration>
        </plugin>
    </plugins>
</build>
```
    
#### 详细的安装和使用文档： 
- https://mapstruct.org/documentation/stable/reference/html/
    
#### MapStruct Examples
- https://github.com/mapstruct/mapstruct-examples

    

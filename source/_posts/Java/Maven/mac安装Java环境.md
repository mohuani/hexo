---
title: "Mac安装Java环境"
date: 2022-02-17 21:22:30
draft: true
tags: [Java, maven]
categories:
- [Java, maven]
---


### JAVA环境

#### 通过源码进行安装
- 找到官方JDK   https://www.oracle.com/java/technologies/javase/javase8-archive-downloads.html
- 下载  ![image](https://user-images.githubusercontent.com/21000558/154419845-afc08cc1-2504-4c1e-a351-da4bc656f179.png)
- GUI程序下一步下一步安装
- 配置环境参数
    ```shell
    #设置环境变量
    vim ~/.zshrc 
    #最后加入，目前大多数文档都是说 JAVA_HOME 配置到这里，但是我 M1电脑  系统 12.1 发现后面配置maven的时候不行，于是 JAVA_HOME 配置成了下面的版本，据说高版本的系统，配置路径修改了
    # export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_171.jdk/Contents/Home 
    
    export JAVA_HOME=$(/usr/libexec/java_home)
    export CLASSPATH=$JAVA_HOME/lib
    export PATH=$JAVA_HOME/bin:$PATH

    #保存退出

    #使配置生效 
    source ~/.zshrc

    #显示刚配置的路径
    echo $JAVA_HOME 

    #查看版本
    java -version
    java version "1.8.0_171"
    Java(TM) SE Runtime Environment (build 1.8.0_171-b11)
    Java HotSpot(TM) 64-Bit Server VM (build 25.171-b11, mixed mode)

    ```
#### 通过 IDEA 直接安装

IDEA :  File - Project Structure - Paltform Setting - SDKs

![image](https://user-images.githubusercontent.com/21000558/154421260-4749a981-309f-4b99-ac87-9d683c4174fb.png)
下面网速随缘，不过应该比官网快


### Maven环境

| 我通过 brew 安装 maven 的时候总是给我又下载一份 OpenJDK，比较乱，我打算自己通过源码进行安装


 - 官方网站：https://maven.apache.org/download.cgi
 - ![image](https://user-images.githubusercontent.com/21000558/154421706-9c276602-82b6-4921-93d1-f392e8a0498c.png)
 - 下载后放到一个合适的路径解压


- 配置环境参数

```shell
  #设置环境变量
  vim ~/.zshrc 

  export M2_HOME=/Users/mohuani/develop/apache-maven-3.8.4
  export PATH=$PATH:$M2_HOME/bin

  #保存退出

  #使配置生效 
  source ~/.zshrc

  #查看版本
  mvn -v
  Apache Maven 3.8.4 (9b656c72d54e5bacbed989b64718c159fe39b537)
  Maven home: /Users/mohuani/develop/apache-maven-3.8.4
  Java version: 1.8.0_202, vendor: Oracle Corporation, runtime: /Library/Internet Plug-Ins/JavaAppletPlugin.plugin/Contents/Home
  Default locale: zh_CN, platform encoding: UTF-8
  OS name: "mac os x", version: "10.16", arch: "x86_64", family: "mac"

```

















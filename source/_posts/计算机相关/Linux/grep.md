---
title: grep
date: 2021-04-08 20:32:47
draft: true
tags: [计算机相关, Linux, grep]
categories:
- [计算机相关, Linux, grep]
---

> 我们平常在linux服务器查询日志文件的时候，经常会使用 `cat xxx.log | grep 'aaa'` 来找寻自己要搜索的东西，但是也会出现这样的场景，我们要搜的东西，不知道落到了哪个目录或者文件下面，所以就需要使用全量模糊搜索

![image-20210408204947514](../../../images/2021/image-20210408204947514.png)

**grep 命令文档** ：https://www.runoob.com/linux/linux-comm-grep.html



### 使用示例

- 以递归的方式查找符合条件的文件。例如，查找指定目录/etc/acpi 及其子目录（如果存在子目录的话）下所有文件中包含字符串"update"的文件，并打印出该字符串所在行的内容，使用的命令为：

  ```shell
  grep -rn update /etc/acpi 
  ```

  


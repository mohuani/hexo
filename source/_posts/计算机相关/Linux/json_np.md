---
title: json_np
date: 2021-04-12 00:32:47
draft: true
tags: [计算机相关, Linux, json_np]
categories:
- [计算机相关, Linux, json_np]
---

> 我们平常在terminal通过curl来调接口检查接口的正确性，默认返回的json内容如果少的话，还可以肉眼直接硬看，但是如果返回的数据如果特别多的话，并且结构层级又比较多，再用肉眼看就不太方便了，一般会把结果复制出来，粘贴到在线的json解析平台看一下内容，其实在命令行也可以直接通过json的格式预览返回内容

上代码
```shell
echo '{"uid":100120,"token":"1fa9fb8004b04f66b7da57393641eddc"}' | json_pp


{
   "token" : "1fa9fb8004b04f66b7da57393641eddc",
   "uid" : 100120
}

```

参考文章：https://www.cnblogs.com/johnnyzen/p/13534405.html
  


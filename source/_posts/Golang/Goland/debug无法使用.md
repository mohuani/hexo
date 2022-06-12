---
title: "M1 goland debug 无法使用"
date: 2022-02-20 11:22:30
draft: true
tags: [golang, goland, debug]
categories:
- [golang, goland, debug]
---


| 背景：Apple M1 芯片使用Goland编码的时候，正常go build 是可以使用的，但是 debug 是置灰的，不能使用， 

![image](https://user-images.githubusercontent.com/21000558/154827585-c78ceed6-73ab-4e14-985a-a4cb99f296b0.png)


### 一、下载 go-delve/delve
- 去github 上面下载 [go-delve/delve](https://github.com/go-delve/delve/blob/master/Documentation/installation/README.md)

```
$ git clone https://github.com/go-delve/delve
$ cd delve
$ go install github.com/go-delve/delve/cmd/dlv

# 会在下面的目录下生成可执行文件  `dlv`
$ cd $GOPATH/bin  

```

### 二、配置 dlv
- goland里面去设置，点开HELP--Edit Custom Properties, 他会提示我们创建一个文件，然后我们在其中添加一行：

```
# custom GoLand properties
dlv.path=你的gopath目录/bin/dlv
```

### 三、重启Goland
- 需要重启一下Goland刚刚的配置才能生效


---
title: golang操作etcd
date: 2021-04-10 13:58:33
tags: [golang, etcd]
categories:
- [golang, etcd]
---

> golang 操作etcd,实现简单的 put，get，delete， watch

> 仓库链接：https://gitee.com/mohuani/golang-demo/tree/master/etcdDemo

```golang
package main

import (
	"context"
	"fmt"
	"go.etcd.io/etcd/client/v3"
	"time"
)

func main() {

	var etcdTestKey = "mohuani-key-1"
	var etcdTestValue = "mohuani-value-1"

	cli, err := clientv3.New(clientv3.Config{
		Endpoints:   []string{"127.0.0.1:2379"},
		DialTimeout: time.Second,
	})

	if err != nil {
		fmt.Println("connect to etcd failed, err: ", err)
		return
	}
	defer cli.Close()

	// put key-value to etcd
	ctx, cancel := context.WithTimeout(context.Background(), time.Second)
	_, err = cli.Put(ctx, etcdTestKey, etcdTestValue)
	if err != nil {
		fmt.Println("put to etcd failed, err: ", err)
		return
	}
	cancel()

	// get value by key from etcd
	ctx, cancel = context.WithTimeout(context.Background(), time.Second)
	getEtcdResult, err := cli.Get(ctx, etcdTestKey)
	if err != nil {
		fmt.Println("get from etcd failed, err: ", err)
		return
	}
	for _, resultValue := range getEtcdResult.Kvs {
		fmt.Printf("key:%s value:%s\n", resultValue.Key, resultValue.Value)
	}
	cancel()

	delResult, err := cli.Delete(context.Background(), etcdTestKey)
	if err != nil {
		fmt.Println("delete from etcd failed, err: ", err)
		return
	}
	fmt.Println(delResult)
	fmt.Println("-----")

	// watch: 监控某个key的操作变化
	watchEtcdTestResults := cli.Watch(context.Background(), etcdTestKey)
	for watchEtcdTestResult := range watchEtcdTestResults {
		for _, evt := range watchEtcdTestResult.Events {
			fmt.Printf("type:%s key: %s value: %s\n", evt.Type.String(), evt.Kv.Key, evt.Kv.Value)
		}
	}

}

```

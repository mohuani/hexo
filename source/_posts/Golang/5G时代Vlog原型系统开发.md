---
title: "5G时代Vlog原型系统开发"
date: 2019-06-15 21:22:30
draft: true
tags: [golang]
categories:
- [golang]
---

##### 5G时代Vlog原型系统开发
课程地址：https://www.imooc.com/learn/1131

##### 课程代码
ps：其中的文件上传不知道哪里出错了，一直不成功

```golang
package main

import (
	"crypto/md5"
	"encoding/json"
	"fmt"
	"io"
	"net/http"
	"os"
	"path/filepath"
	"strings"
	"time"
)

func sayHello(w http.ResponseWriter, r *http.Request) {
	w.Write([]byte("hello world"))
}

func main() {

	fileHandler := http.FileServer(http.Dir("./video"))

	http.Handle("/video/", http.StripPrefix("/video/", fileHandler))

	http.HandleFunc("/api/upload", uploadHandler)

	http.HandleFunc("/api/list", getFileListHandler)

	http.HandleFunc("/sayHello", sayHello)

	http.ListenAndServe(":8090", nil)
}

/**
上传文件
*/
func uploadHandler(w http.ResponseWriter, r *http.Request) {
	r.Body = http.MaxBytesReader(w, r.Body, 10*1024*1024)
	err := r.ParseMultipartForm(10 * 1024 * 1024)
	if err != nil {
		http.Error(w, err.Error(), http.StatusInternalServerError)
		return
	}

	file, fileHeader, err := r.FormFile("uploadFile")
	ret := strings.HasSuffix(fileHeader.Filename, ".mp4")
	if ret == false {
		http.Error(w, "not mp4", http.StatusInternalServerError)
		return
	}

	md5Byte := md5.Sum([]byte(fileHeader.Filename + time.Now().String()))
	md5Str := fmt.Sprintf("%x", md5Byte)
	newFileName := md5Str + ".mp4"

	dst, err := os.Create("./video/" + newFileName)
	defer dst.Close()
	if err != nil {
		http.Error(w, err.Error(), http.StatusInternalServerError)
		return
	}
	defer file.Close()
	if _, err := io.Copy(dst, file); err != nil {
		http.Error(w, err.Error(), http.StatusInternalServerError)
		return
	}
	return
}

func getFileListHandler(w http.ResponseWriter, r *http.Request) {
	files, _ := filepath.Glob("video/*")
	var ret []string
	for _, file := range files {
		ret = append(ret, "http://"+r.Host+filepath.Base(file))
	}
	retJson, _ := json.Marshal(ret)
	w.Write(retJson)
	return
}

```
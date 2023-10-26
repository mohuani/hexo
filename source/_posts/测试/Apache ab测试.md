---
title: "Apache ab测试"
date: 2020-04-08 20:32:47
draft: true
tags: [测试]
categories:
- [测试]
---



 - AB测试中常见的命令
```
ab -help
Usage: ab [options] [http[s]://]hostname[:port]/path
Options are:
    -n requests     Number of requests to perform
    -c concurrency  Number of multiple requests to make
    -t timelimit    Seconds to max. wait for responses
    -b windowsize   Size of TCP send/receive buffer, in bytes
    -p postfile     File containing data to POST. Remember also to set -T
    -u putfile      File containing data to PUT. Remember also to set -T
    -T content-type Content-type header for POSTing, eg.
                    'application/x-www-form-urlencoded'
                    Default is 'text/plain'
    -v verbosity    How much troubleshooting info to print
    -w              Print out results in HTML tables
    -i              Use HEAD instead of GET
    -x attributes   String to insert as table attributes
    -y attributes   String to insert as tr attributes
    -z attributes   String to insert as td or th attributes
    -C attribute    Add cookie, eg. 'Apache=1234. (repeatable)
    -H attribute    Add Arbitrary header line, eg. 'Accept-Encoding: gzip'
                    Inserted after all normal header lines. (repeatable)
    -A attribute    Add Basic WWW Authentication, the attributes
                    are a colon separated username and password.
    -P attribute    Add Basic Proxy Authentication, the attributes
                    are a colon separated username and password.
    -X proxy:port   Proxyserver and port number to use
    -V              Print version number and exit
    -k              Use HTTP KeepAlive feature
    -d              Do not show percentiles served table.
    -S              Do not show confidence estimators and warnings.
    -g filename     Output collected data to gnuplot format file.
    -e filename     Output CSV file with percentages served
    -r              Don't exit on socket receive errors.
    -h              Display usage information (this message)
    -Z ciphersuite  Specify SSL/TLS cipher suite (See openssl ciphers)
    -f protocol     Specify SSL/TLS protocol (SSL2, SSL3, TLS1, or ALL)
```
 - 吞吐率（Requests per second）
服务器并发处理能力的量化描述，单位是reqs/s，指的是在某个并发用户数下单位时间内处理的请求数。某个并发用户数下单位时间内能处理的最大请求数，称之为最大吞吐率。
note：吞吐率是基于并发用户数的。这句话代表了两个含义：
a、吞吐率和并发用户数相关
b、不同的并发用户数下，吞吐率一般是不同的
计算公式：总请求数/处理完成这些请求数所花费的时间，即

```
Request per second=Complete requests/Time taken for tests
```
 - 并发连接数（The number of concurrent connections）
并发连接数指的是某个时刻服务器所接受的请求数目，简单的讲，就是一个会话。

 - 并发用户数（Concurrency Level）
要注意区分这个概念和并发连接数之间的区别，一个用户可能同时会产生多个会话，也即连接数。在HTTP/1.1下，IE7支持两个并发连接，IE8支持6个并发连接，FireFox3支持4个并发连接，所以相应的，我们的并发用户数就得除以这个基数。


 - 用户平均请求等待时间（Time per request）
计算公式：处理完成所有请求数所花费的时间/（总请求数/并发用户数），即：
Time per request=Time taken for tests/（Complete requests/Concurrency Level）


 - 服务器平均请求等待时间（Time per request:across all concurrent requests）

 **下面是对慕课网首页 https://www.imooc.com/ 做的一个简单的测试**

```
ab -c 10 -n 100 https://www.imooc.com/
  -c concurrency  Number of multiple requests to make   表示并发用户数为10
  -t timelimit    Seconds to max. wait for responses    表示请求总数为100
```


```
$ ab -c 10 -n 100 https://www.imooc.com/
This is ApacheBench, Version 2.3 <$Revision: 655654 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking www.imooc.com (be patient).....done


Server Software:        nginx
Server Hostname:        www.imooc.com
Server Port:            443
SSL/TLS Protocol:       TLSv1/SSLv3,ECDHE-RSA-AES128-GCM-SHA256,2048,128

Document Path:          /
Document Length:        249325 bytes

Concurrency Level:      10
Time taken for tests:   5.677 seconds
Complete requests:      100
Failed requests:        0
Write errors:           0
Total transferred:      25586992 bytes
HTML transferred:       25554135 bytes
Requests per second:    17.62 [#/sec] (mean)
Time per request:       567.674 [ms] (mean)
Time per request:       56.767 [ms] (mean, across all concurrent requests)
Transfer rate:          4401.70 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:      106  201 244.8    123    1124
Processing:   213  304 179.8    256    1437
Waiting:       64  110 109.8     77     624
Total:        322  505 298.0    388    1550

Percentage of the requests served within a certain time (ms)
  50%    388
  66%    430
  75%    449
  80%    470
  90%    885
  95%   1366
  98%   1498
  99%   1550
 100%   1550 (longest request)
```
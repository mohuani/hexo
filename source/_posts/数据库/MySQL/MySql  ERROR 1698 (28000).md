---
title: "MySql  ERROR 1698 (28000)"
date: 2019-10-18 20:36:39
draft: true
tags: [数据库, MySQL]
categories:
- [数据库, MySQL]
---

MySql 5.7.6 起，安装时如果 root 不设置密码，那么默认会采用auth_socket的方式登陆 MySQL 。也就是登陆 MySQL 时验证你的 Linux 的当前用户是否为 root，如果不是就不能登陆。在`auth_socket`模式下，应用程序通过数据库的用户名、密码是无法连接的，这就需要我们将数据库的登陆模式，改为`mysql_native_password`模式。

进入数据库：sudo mysql -uroot，执行：

```bash
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '';
FLUSH PRIVILEGES;
exit;
```
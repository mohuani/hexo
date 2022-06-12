---
title: "PHP设计模式（适配器模式）"
date: 2017-12-04 21:22:30
draft: true
tags: [PHP]
categories:
- [PHP]
---



#### 设计动机
假如我们又这样软件系统，我们希望它能够和一个新的库搭配使用，但是这个库所提供的接口与我们的软件系统不兼容，我们不想改变现有代码就能解决这个问题，怎么办？这个时候我们就需要将这个新的库接口转换成我们所需要的接口，这就是适配器模式设计动机。

#### 模式定义

 1. 适配器模式就是将一个类的接口，转换成客户期望的另一个接口。适配器让原本接口不兼容的类可以合作无间。

 2. 在适配器模式中，我们可以定义一个包装类，包装不兼容接口的对象，这个包装类就是适配器，它所包装的对象就是适配者。
    
 3. 适配器提供给客户需要的接口，适配器的实现就是将客户的请求转换成对适配者的相应的接口的引用。也就是说，当客户调用适配器的方法时，适配器方法内部将调用适配者的方法，客户并不是直接访问适配者的，而是通过调用适配器方法访问适配者。因为适配器可以使互不兼容的类能够“合作愉快”。

#### 代码实现 

 **1. 首先我们定义一个数据库操作对外，公用的API接口，里面主要设计了connect()   query()   close()  方法。**

```php
<?php
namespace IMooc;

interface IDatabase
{
    function connect($host, $user, $passwd, $dbname);
    function query($sql);
    function close();
}
```
 **2. 使用mysql实现**
```
<?php
namespace IMooc\Database;

use IMooc\IDatabase;

class MySQL implements IDatabase
{
    protected $conn;
    function connect($host, $user, $passwd, $dbname)
    {
        $conn = mysql_connect($host, $user, $passwd);
        mysql_select_db($dbname, $conn);
        $this->conn = $conn;
    }

    function query($sql)
    {
        $res = mysql_query($sql, $this->conn);
        return $res;
    }

    function close()
    {
        mysql_close($this->conn);
    }
}
```

 **3. 使用mysqli实现**


```
<?php
namespace IMooc\Database;

use IMooc\IDatabase;

class MySQLi implements IDatabase
{
    protected $conn;

    function connect($host, $user, $passwd, $dbname)
    {
        $conn = mysqli_connect($host, $user, $passwd, $dbname);
        $this->conn = $conn;
    }

    function query($sql)
    {
        return mysqli_query($this->conn, $sql);
    }

    function close()
    {
        mysqli_close($this->conn);
    }
}
```

 **4. 使用PDO实现** 


```
<?php
namespace IMooc\Database;

use IMooc\IDatabase;

class PDO implements IDatabase
{
    /**
     * @var \PDO
     */
    protected $conn;
    function connect($host, $user, $passwd, $dbname)
    {
        $conn = new \PDO("mysql:host=$host;dbname=$dbname", $user, $passwd);
        $this->conn = $conn;
    }

    function query($sql)
    {
        return $this->conn->query($sql);
    }

    function close()
    {
        unset($this->conn);
    }
}
```
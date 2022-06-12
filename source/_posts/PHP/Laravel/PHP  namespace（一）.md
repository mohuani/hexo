---
title: "PHP  namespace"
date: 2015-12-04 21:22:30
draft: true
tags: [PHP, Laravel]
categories:
- [PHP, Laravel]
---

#命名空间的作用域



### **1.函数的namespace**

 - 各自命名空间调用各自命名空间的函数

```php
<?php

namespace a
{
    function hello()
    {
        return '命名空间' . __NAMESPACE__ . '<br>函数名称是：' . __FUNCTION__;
    }
}

namespace b
{
    function hello()
    {
        return '命名空间' . __NAMESPACE__ . '<br>函数名称是：' . __FUNCTION__;
    }
}

namespace
{
    echo a\hello();     //调用a空间中的hello
    echo '<hr>';
    echo b\hello();     //调用b空间中的hello
}

?>
```

运行的结果

![这里写图片描述](https://img-blog.csdn.net/20180410160251563?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)


 - 还可以在b的命名空间中调用a命名空间中的函数

```php
<?php

namespace a
{
    function hello()
    {
        return '命名空间' . __NAMESPACE__ . '<br>函数名称是：' . __FUNCTION__;
    }
}

namespace b
{
    function hello()
    {
        return \a\hello();
    }
}

namespace
{
    echo a\hello();     //调用a空间中的hello
    echo '<hr>';
    echo b\hello();     //b调用a空间中的hello
}



?>
```

运行结果

![这里写图片描述](https://img-blog.csdn.net/20180410161125940?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)


### **2.类的命名空间**

 - 各自命名空间调用各自命名空间的函数


```php
<?php

namespace a
{
    class A
    {
        public $name = 'mohuani';
        public function say()
        {
            $namespace = '命名空间：' . __NAMESPACE__;
            $className = '类名' . __CLASS__;
            $methodName = '方法名' . __METHOD__;
            return $namespace . '<br>' . $className . '<br>' . $methodName . '<br>' . $this->name;
        }
    }
}



namespace b
{
    class A
    {
        public $name = 'mohuani';
        public function say()
        {
            $namespace = '命名空间：' . __NAMESPACE__;
            $className = '类名' . __CLASS__;
            $methodName = '方法名' . __METHOD__;
            return $namespace . '<br>' . $className . '<br>' . $methodName . '<br>' . $this->name;
        }
    }
}

namespace
{
    echo (new a\A)->say();
    echo '<hr>';
    echo (new b\A)->say();
}



?>

```

运行结果

![这里写图片描述](https://img-blog.csdn.net/20180410162810247?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

 - 在b的空间中调用a空间中的类


```php
<?php

namespace a
{
    class A
    {
        public $name = '陌花拟';
        public function say()
        {
            $namespace = '命名空间：' . __NAMESPACE__;
            $className = '类名' . __CLASS__;
            $methodName = '方法名' . __METHOD__;
            return $namespace . '<br>' . $className . '<br>' . $methodName . '<br>' . $this->name;
        }
    }
}



namespace b
{
    class A
    {
        public $name = 'mohuani';
        public function say()
        {
            $namespace = '命名空间：' . __NAMESPACE__;
            $className = '类名' . __CLASS__;
            $methodName = '方法名' . __METHOD__;
            $temp = (new \a\A)->name;
            return $namespace . '<br>' . $className . '<br>' . $methodName . '<br>' . $temp;
        }
    }
}

namespace
{
    echo (new a\A)->say();
    echo '<hr>';
    echo (new b\A)->say();
}



?>

```

运行结果

![这里写图片描述](https://img-blog.csdn.net/20180410163339119?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

###**3.常量的命名空间**

 - const创建的常量，受命名空间的限制 
 - define创建的常量，不受命名空间的限制

```php
<?php

namespace a
{
    const SITE_NAME = '陌花拟';
    //define('SITE_NAME','陌花拟')
    //define创建的常量，不收命名空间的限制
}


namespace b
{
    const SITE_NAME = 'mohuani';
}

namespace
{
    echo a\SITE_NAME;
    echo '<hr>';
    echo b\SITE_NAME;
}


?>
```
运行结果

![这里写图片描述](https://img-blog.csdn.net/20180410164321969?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dmazI5NzUwMTk2NzE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)



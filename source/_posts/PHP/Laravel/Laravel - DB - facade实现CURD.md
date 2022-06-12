---
title: "Laravel - DB - facade实现CURD"
date: 2015-12-04 21:22:30
draft: true
tags: [PHP, Laravel]
categories:
- [PHP, Laravel]
---

新建的数据表SQL

```sql
CREATE TABLE IF NOT EXISTS students(
    `id` INT AUTO_INCREMENT PRIMARY KEY,
    `name` VARCHAR(255) NOT NULL DEFAULT '' COMMENT '姓名',
    `age` TINYINT UNSIGNED NOT NULL DEFAULT 0 COMMENT '年龄',
    `sex` TINYINT UNSIGNED NOT NULL DEFAULT 10 COMMENT '性别',
    `created_at` INT NOT NULL DEFAULT 0 COMMENT '新增时间',
    `updated_at` INT NOT NULL DEFAULT 0 COMMENT '修改时间'
)ENGINE=InnoDB DEFAULT CHARSET=UTF8 AUTO_INCREMENT=1001 COMMENT='学生表';
```

在控制器中实现基本的CURD

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Support\Facades\DB;

class StudentController extends Controller
{

    public function test1()
    {
	    //查询操作
        $student = DB::select('select * from student');
        var_dump($student);
		
		//插入操作
        $bool = DB::insert('insert into student (name, age) values (?, ?)',
            ['imooc', 19]);
        var_dump($bool);
		
		//更新操作
        $num = DB::update('update student set age = ? where name = ?',
            [20, 'wfk']);
        var_dump($num);

		//查询操作
        $student = DB::select('select * from student where id > ?',
            [1001]);
        var_dump($student);
        dd($student);

		//删除操作
        $num = DB::delete('delete from student where id > ?', [1001]);
        var_dump($num);
    }
}
```
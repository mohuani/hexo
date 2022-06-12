---
title: "Laravel 查询构造器的使用（一）"
date: 2015-12-04 21:22:30
draft: true
tags: [PHP, Laravel]
categories:
- [PHP, Laravel]
---

使用查询构造器对数据库的 增 - 删 - 改 操作

```php
	//使用查询构造器新增数据
    public function query1()
    {
        $bool = DB::table('student')->insert(
            ['name' => 'mohuani', 'age' => 19]
        );
        var_dump($bool);

        $id = DB::table('student')->insertGetId(
            ['name' => 'sean', 'age' => 18]
        );
        var_dump($id);


        $id = DB::table('student')->insert([
            ['name' => 'name1', 'age' => 21],
            ['name' => 'name2', 'age' => 22]
        ]);
        var_dump($id);

    }


    //使用查询构造器更新数据
    public function query2()
    {
        $sum = DB::table('student')
            ->where('id', 1001)
            ->update(['age' => 30]);
        var_dump($sum);

        //实现自增
        $sum = DB::table('student')->increment('age');
        $sum = DB::table('student')->increment('age', 3);

        //实现自减
        $sum = DB::table('student')->decrement('age');
        $sum = DB::table('student')->decrement('age', 3);

        //使用where条件更新
        $num = DB::table('student')
            ->where('id', 1004)
            ->decrement('age', 3);
        var_dump($num);

        //使用where条件更新
        $num = DB::table('student')
            ->where('id', 1004)
            ->decrement('age', 3, ['name' => 'iimooc']);
        var_dump($num);
    }

    //使用查询构造器删除数据
    public function query3()
    {
        $num = DB::table('student')
            ->where('id',1008)
            ->delete();
        var_dump($num);

        $num = DB::table('student')
            ->where('id', '>=', 1008)
            ->delete();
        var_dump($num);

        DB::table('student')->truncate();
    }
```
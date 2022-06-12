---
title: "Laravel 查询构造器的使用（二）"
date: 2015-12-04 21:22:30
draft: true
tags: [PHP, Laravel]
categories:
- [PHP, Laravel]
---

使用查询构造器对数据库的 查询 操作
```php
//使用查询构造器查询数据
    public function query4()
    {
        //get()取出所有的数据
        $student = DB::table('student')->get();
        dd($student);

        //first()取出结果集中的第一条数据
        $student = DB::table('student')->first();
        dd($student);

        $student = DB::table('student')
            ->orderBy('id', 'desc')
            ->first();
        dd($student);
        
        //单个where()条件
        $student = DB::table('student')
            ->where('id', '>=', 1002)
            ->get();
        dd($student);

        //多个whereRaw()条件
        $student = DB::table('student')
            ->whereRaw('id >= ? and age  > ?', [1002, 20])
            ->get();
        dd($student);

        //pluck()返回具体的字段
        $name = DB::table('student')
            ->pluck('name');
        dd($name);

        //lists()效果和pluck()类似
        $name = DB::table('student')
            ->lists('name', 'id');
        dd($name);

        //select()
        $student = DB::table('student')
            ->select('id' ,'name', 'age')
            ->get();
        dd($student);

        //chunk()一次查询几条数据
        echo '<pre>';
        DB::table('student')->chunk(2, function ($student){
            var_dump($student);
        });
    }
```
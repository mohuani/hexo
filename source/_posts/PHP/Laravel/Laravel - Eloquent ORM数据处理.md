---
title: "Laravel - Eloquent ORM数据处理"
date: 2015-12-04 21:22:30
draft: true
tags: [PHP, Laravel]
categories:
- [PHP, Laravel]
---

#Laravel - Eloquent ORM数据处理


- 将数据库和Model进行绑定

```
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Student extends Model
{
    // 指定表名
    protected $table = 'student';

    // 指定id
    protected $primaryKey = 'id';

    // 指定允许批量赋值的字段
    protected $fillable = ['name', 'age'];

    // 指定不允许批量赋值的字段
    protected $guarded = [];

    //自动维护时间戳
    public $timestamp = true;

    protected function getDateFormat()
    {
        return time();
    }

    protected function asDateTime($val)
    {
        return $val;
    }
}
```

 **1. 使用模型查询数据**

```
    public function orm1()
    {
        //all()
        $student = Student::all();
        dd($student);

        //find()
        Student::find(1001);
        dd($student);

        //findOrFail()
        $student = Student::findOrFail(1001);
        dd($student);

    }
```
**2.使用模型新增数据**
```
    public function orm2()
    {
        //使用模型新增数据
        $student = new Student();
        $student->name = 'sean1';
        $student->age = 19;
        $bool = $student->save();
        dd($bool);

        $student = Student::find(1012);
        echo $student->created_at;
        echo date('Y-m-d H:i:s');

        //使用模型的Create()方法新增数据
        $student = Student::create(
            ['name' => 'imooc', 'age' => 18]
        );
        dd($student);

        //firstOrCreate()
        $student = Student::firstOrCreate(
            ['name' => 'imoocs', 'age' => 18]
        );
        dd($student);

        //firstOrNew()
        $student = Student::firstOrNew(
            ['name' => 'imoocss', 'age' => 18]
        );
        $bool = $student->save();
        dd($student);

    }
```
**3.通过模型更新数据**
```
    public function orm3()
    {
        //通过模型更新数据
        $student = Student::find(1014);
        $student->name = 'kitty';
        $bool = $student->save();
        var_dump($bool);

        $sum = Student::where('id', '>', 1013)->update(
            ['age' => 32]
        );
        var_dump($sum);
    }
```

**4.通过模型删除数据**
```
    public function orm4()
    {
        //通过主键查找
        $bool = Student::find(1005)->delete();
        dd($bool);

        //通过主键删除税数据
        $num = Student::detroy(1013);
        $num = Student::detroy(1013, 1014);
        $num = Student::detroy([1013,1014]);
        var_dump($num);

        //通过where条件删除数据
        $num = Student::where('id', '>', '1007')->delete();
        var_dump($num);

    }
```
---
title: "Laravel collecte break continue"
date: 2020-12-04 21:22:30
draft: true
tags: [PHP, Laravel, Collection]
categories:
- [PHP, Laravel, Collection]
---

---
typora-root-url: ../images
---

[Laravel collecte break continue](https://editor.csdn.net/md/?articleId=114985626)

> 背景： Laravel框架中循环我们都推荐使用 collecte 进行循环，但是如果我们想要在循环中
> break 或者 continue，直接break或者continue，语法层面会直接报错，那么怎么才能实现上述所要的效果呢。其实在循环中 `return`的效果就类似与`break`，而 `return false` 的效果就类似于 `continue`

测试demo如下
```php

<?php

use Illuminate\Database\Seeder;

class MohuaniSeeder extends Seeder
{
    /**
     * Run the database seeds.
     *
     * @return void
     */
    public function run()
    {
        $this->collectContinue();
        $this->command->info("--------------");
        $this->collectBreak();
    }

    public function collectContinue () {
        $arrList = [
            "a" => "A",
            "b" => "B",
            "c" => "C",
        ];

        $this->command->info(111);
        collect($arrList)->each(function ($arr) {
            $this->command->info(222);
            if ($arr == "b") {
                $this->command->info(333);
                return; // 跳出本次循环，类似continue
            }
        });

        $this->command->info(444);
    }
    public function collectBreak () {
        $arrList = [
            "a" => "A",
            "b" => "B",
            "c" => "C",
        ];

        $this->command->info(111);
        collect($arrList)->each(function ($arr) {
            $this->command->info(222);
            if ($arr == "b") {
                $this->command->info(333);
                return false; // 跳出所有循环，类似break
            }
        });

        $this->command->info(444);
    }
}


```

运行seeder
```shell
 php artisan  db:seed  --class=MohuaniSeeder
```

结果：



![20210318194853117](../images/20210318194853117.png)
---
title: "Laravel Seeder"
date: 2021-04-13 21:22:30
draft: true
tags: [PHP, Laravel]
categories:
- [PHP, Laravel]
---

1、创建Seeder
运行 `Artisan` 命令 `make:seeder` 生成 Seeder，框架生成的 seeders 都放在 `database/seeds` 目录下：
```php
php artisan make:seeder UserSeeder
```
调用其它 Seeders#
在 `DatabaseSeeder` 类中，你可以使用 `call` 方法来运行其它的 seed 类。使用 `call` 方法可以将数据填充拆分成多个文件，这样就不会使单个 seeder 变得非常大。只需简单传递要运行的 seeder 类名称即可：

```php
/**
 * 执行数据库填充
 *
 * @return void
 */
public function run()
{
    $this->call([
        UserSeeder::class,
        PostSeeder::class,
        CommentSeeder::class,
    ]);
}
```

2、运行Seeder

你可以使用 `Artisan` 命令 `db:seed` 来填充数据库了。默认情况下，`db:seed` 命令将运行 `DatabaseSeeder` 类，这个类可以用来调用其它 Seed 类。不过，你也可以使用 `--class` 选项来指定一个特定的 seeder 类：

```php

composer dump-autoload

php artisan db:seed

php artisan db:seed --class=UserSeeder

```








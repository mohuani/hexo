---
title: "Laravel - ide-helper"
date: 2021-07-01 15:22:30
draft: true
tags: [PHP, Laravel]
categories:
- [PHP, Laravel]
---

通过 `ide-helper` 自动生成Model上面的 属性

```shell
php artisan ide-helper:models "App\\Models\\CustomizedSkuInfo"
```

Model层最好指定一下表名，并且数据库中这张表已经存在

```php
class CustomizedSkuInfo extends Model {
    use SoftDeletes;

    protected $table = 'customized_sku_info';
}
```

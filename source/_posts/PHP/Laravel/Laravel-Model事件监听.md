---
title: "Laravel-Model事件监听"
date: 2021-05-20 22:22:30
draft: true
tags: [PHP, Laravel]
categories:
- [PHP, Laravel]
---



> 通常我们有这样的业务需求，如果Model的某个字段发生了变化，就做出对应的业务处理，那么怎么才能监测到字段的变化呢，Laravel的Eloquent Model模型为我们提供了一套解决方案。其实就是利用Observer监听Model。

执行命令
```shell
1、创建事件

php artisan make:observer AfterSaleObserver --model=Models/User

2、在 AppServiceProvider 里面注册事件

```
在 `Observers`目录下回生成 `UserObserver`文件

### 事件
Eloquent 的模型触发了几个事件，可以在模型的生命周期的以下几点进行监控： `retrieved、creating、created、updating、updated、saving、saved、deleting、deleted、restoring、restored`。事件能在每次在数据库中保存或更新特定模型类时轻松地执行代码。
从数据库中检索现有模型时会触发 `retrieved` 事件。当新模型第一次被保存时， `creating` 以及 `created` 事件会被触发。如果模型已经存在于数据库中并且调用了 `save` 方法，会触发 `updating` 和 `updated` 事件。在这两种情况下，`saving / saved` 事件都会触发。
开始前，在 Eloquent 模型上定义一个 `$dispatchesEvents` 属性，将 Eloquent 模型的生命周期的各个点映射到你的 事件类 中。

```php
<?php

namespace App;

use App\Events\UserSaved;
use App\Events\UserDeleted;
use Illuminate\Notifications\Notifiable;
use Illuminate\Foundation\Auth\User as Authenticatable;

class User extends Authenticatable
{
    use Notifiable;

    /**
     * 模型的事件映射。
     *
     * @var array
     */
    protected $dispatchesEvents = [
        'saved' => UserSaved::class,
        'deleted' => UserDeleted::class,
    ];
}
```

### 观察器

如果要给某个模型监听很多事件，则可以使用观察器将所有监听器分组到一个类中。观察器类里的方法名应该对应 Eloquent 中你想监听的事件。 每种方法接收 model 作为其唯一的参数。Laravel 没有为观察器设置默认的目录，所以你可以创建任何你喜欢你的目录来存放：


```php
<?php

namespace App\Observers;

use App\User;

class UserObserver
{
    /**
     * 监听用户创建的事件。
     *
     * @param  User  $user
     * @return void
     */
    public function created(User $user)
    {
        //
        $user->getOriginal('status');
        $user->getOriginal();
    }
    
    /**
     * Handle the after sale "updated" event.
     *
     * @param AfterSale $afterSale
     * @return void
     */
    public function updated (User $user) {

        $newStatus = $user->status;  // 获取model更新后的字段值
        $oldStatus = $user->getOriginal('status');   // 获取model更新前的字段值
        $oldUser = $user->getOriginal();  // 获取更新前的整个model实例

        if ($newStatus != $oldStatus) {
            // todo something
            Logger::info($user->getOriginal());
        }

    }

    /**
     * 监听用户删除事件。
     *
     * @param  User  $user
     * @return void
     */
    public function deleting(User $user)
    {
        //
    }
}
```

要注册一个观察器，需要在模型上使用 `observe` 方法。你可以在服务提供器中的 `boot` 方法注册观察器。在这个例子中，我们将在 `AppServiceProvider` 注册观察器：

```php
<?php

namespace App\Providers;

use App\User;
use App\Observers\UserObserver;
use Illuminate\Support\ServiceProvider;

class AppServiceProvider extends ServiceProvider
{
    /**
     * 运行所有应用.
     *
     * @return void
     */
    public function boot()
    {
        User::observe(UserObserver::class);
    }

    /**
     * 注册服务提供.
     *
     * @return void
     */
    public function register()
    {
        //
    }
}


```






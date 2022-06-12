---
title: "Laravel路由执行原理"
date: 2015-12-04 21:22:30
draft: true
tags: [PHP, Laravel]
categories:
- [PHP, Laravel]
---


# Laravel 路由执行原理


参考文档：

https://learnku.com/articles/13622/the-principle-of-laravel-routing-execution#955cd9

https://laworigin.github.io/2018/12/28/laravel路由解析分析/

我自己做了一个ppt: [Laravel路由执行原理.pptx](https://github.com/mohuani/mohuani.github.io/files/8168769/Laravel.pptx)

示例 laravel 版本 Laravel Framework 8.70.2

- 路由信息的加载过程
- 路由信息的分发处理流程

### 一、路由加载原理

- 原生路由：执行某个class的某个 function

```
curl index.php?con=WeatherInfoByCityKey&act=getWeatherByCityKey

$con = isset($_REQUEST['con']) ? $_REQUEST['con'] : '';
$act = isset($_REQUEST['act']) ? $_REQUEST['act'] : '';

switch ($con) {
    case 'WeatherInfoByCityKey':
        $con = new App\Controllers\WeatherInfoByCityKeyController();
        break;
    ...
}

$con->$act();
```

- Laravel的路由入口

```
RouteServiceProvider

		/**
     * Define your route model bindings, pattern filters, etc.
     *
     * @return void
     */
    public function boot()
    {
        $this->configureRateLimiting();

        $this->routes(function () {
            Route::prefix('api')
                ->middleware('api')
                ->namespace($this->namespace)
                ->group(base_path('routes/api.php'));

            Route::middleware('web')
                ->namespace($this->namespace)
                ->group(base_path('routes/web.php'));
        });
    }
routes/api.php

Route::middleware('auth:sanctum')->get('/user', function (Request $request) {
    return $request->user();
});
```

 **RouteServiceProvider 服务提供者**

简单介绍下「服务提供者」的加载和执行过程：

src/Illuminate/Foundation/Application.php  register()

- 首先，HTTP 内核程序会去执行所有「服务提供者」 register 方法，将所有的服务注册到服务容器内，这里的注册指的是将服务绑定（bind）到容器；
- 当所有「服务提供者」注册完后，会执行已完成注册「服务提供者」的 boot 方法启动服务。

```
Illuminate\\\\Foundation\\\\Support\\\\Providers\\\\RouteServiceProvider

	  /**
     * Register any application services.
     *
     * @return void
     */
    public function register()
    {
        $this->booted(function () {
            $this->setRootControllerNamespace();

            if ($this->routesAreCached()) {
                $this->loadCachedRoutes();
            } else {
                $this->loadRoutes();

                $this->app->booted(function () {
                    $this->app['router']->getRoutes()->refreshNameLookups();
                    $this->app['router']->getRoutes()->refreshActionLookups();
                });
            }
        });
    }
```

### 深入研究 map 定义路由系列方法

### 二、路由分发

HTTP 请求如何被分发到相关路由并执行路由

### 接收 HTTP 请求

所有的 HTTP 请求操作被入口文件 **public/index.php** 捕获后，交给 **[Illuminate\Foundation\Http\kernel::class](https://github.com/laravel/framework/blob/5.6/src/Illuminate/Foundation/Http/Kernel.php)**  内核处理，**handle** 处理器接收用户的 **Request** 作为参数，然后去执行。

```
public/index.php

$kernel = $app->make(Kernel::class);
$response = $kernel->handle(
    $request = Request::capture()
)->send();
$kernel->terminate($request, $response);
```

### 由 HTTP 内核处理 HTTP 请求

```
src/Illuminate/Foundation/Http/Kernel.php

	  /**
     * Handle an incoming HTTP request.
     *
     * @param  \\\\Illuminate\\\\Http\\\\Request  $request
     * @return \\\\Illuminate\\\\Http\\\\Response
     */
    public function handle($request)
    {
        try {
            $request->enableHttpMethodParameterOverride();

            $response = $this->sendRequestThroughRouter($request);
        } catch (Throwable $e) {
            $this->reportException($e);

            $response = $this->renderException($request, $e);
        }

        $this->app['events']->dispatch(
            new RequestHandled($request, $response)
        );

        return $response;
    }

		/**
     * Send the given request through the middleware / router.
     *
     * @param  \\\\Illuminate\\\\Http\\\\Request  $request
     * @return \\\\Illuminate\\\\Http\\\\Response
     */
    protected function sendRequestThroughRouter($request)
    {
        $this->app->instance('request', $request);

        Facade::clearResolvedInstance('request');

        $this->bootstrap();

        return (new Pipeline($this->app))
                    ->send($request)
                    ->through($this->app->shouldSkipMiddleware() ? [] : $this->middleware)
                    ->then($this->dispatchToRouter());
    }
```

- 清空已解析的请求（clearResolvedInstance）；
- 执行应用的引导程序（bootstrap），这部分的内容请查阅 深入剖析 Laravel 服务提供者实现原理 的服务提供者启动原理小结。
- 将请求发送到中间件和路由中，这个由管道组件完成（Pipeline）。

### 路由分发处理

`then($this->dispatchToRouter())` 路由处理了，让我们看看 `dispatchToRouter` 是如何分发路由的。

```
src/Illuminate/Foundation/Http/Kernel.php

protected function dispatchToRouter(){
    return function ($request) {
        $this->app->instance('request', $request);
        return $this->router->dispatch($request);
    };
}
src/Illuminate/Routing/Router.php

/**
 * @mixin \\\\Illuminate\\\\Routing\\\\RouteRegistrar
 */
class Router implements BindingRegistrar, RegistrarContract
{

    /**
     * Dispatch the request to the application.
     *
     * @param  \\\\Illuminate\\\\Http\\\\Request  $request
     * @return \\\\Symfony\\\\Component\\\\HttpFoundation\\\\Response
     */
    public function dispatch(Request $request)
    {
        $this->currentRequest = $request;

        return $this->dispatchToRoute($request);
    }

 /**
     * Dispatch the request to a route and return the response.
     *
     * @param  \\\\Illuminate\\\\Http\\\\Request  $request
     * @return \\\\Symfony\\\\Component\\\\HttpFoundation\\\\Response
     */
    public function dispatchToRoute(Request $request)
    {
        return $this->runRoute($request, $this->findRoute($request));
    }

    /**
     * Find the route matching a given request.
     *
     * @param  \\\\Illuminate\\\\Http\\\\Request  $request
     * @return \\\\Illuminate\\\\Routing\\\\Route
     */
    protected function findRoute($request)
    {
        $this->current = $route = $this->routes->match($request);

        $route->setContainer($this->container);

        $this->container->instance(Route::class, $route);

        return $route;
    }

    /**
     * Return the response for the given route.
     *
     * @param  \\\\Illuminate\\\\Http\\\\Request  $request
     * @param  \\\\Illuminate\\\\Routing\\\\Route  $route
     * @return \\\\Symfony\\\\Component\\\\HttpFoundation\\\\Response
     */
    protected function runRoute(Request $request, Route $route)
    {
        $request->setRouteResolver(function () use ($route) {
            return $route;
        });

        $this->events->dispatch(new RouteMatched($route, $request));

        return $this->prepareResponse($request,
            $this->runRouteWithinStack($route, $request)
        );
    }

    /**
     * Run the given route within a Stack "onion" instance.
     *
     * @param  \\\\Illuminate\\\\Routing\\\\Route  $route
     * @param  \\\\Illuminate\\\\Http\\\\Request  $request
     * @return mixed
     */
    protected function runRouteWithinStack(Route $route, Request $request)
    {
        $shouldSkipMiddleware = $this->container->bound('middleware.disable') &&
                                $this->container->make('middleware.disable') === true;

        $middleware = $shouldSkipMiddleware ? [] : $this->gatherRouteMiddleware($route);

        return (new Pipeline($this->container))
                        ->send($request)
                        ->through($middleware)
                        ->then(function ($request) use ($route) {
                            return $this->prepareResponse(
                                $request, $route->run()
                            );
                        });
    }

    /**
     * Create a response instance from the given value.
     *
     * @param  \\\\Symfony\\\\Component\\\\HttpFoundation\\\\Request  $request
     * @param  mixed  $response
     * @return \\\\Symfony\\\\Component\\\\HttpFoundation\\\\Response
     */
    public function prepareResponse($request, $response)
    {
        return static::toResponse($request, $response);
    }

}
```

### 路由分发流程

1、从 RouteCollection 路由集合中查找出当前请求 URI（$request）匹配的路由，由 Router::findRoute($request) 方法完成； 2、运行路由配置阶段所配置的闭包（或控制器方法），这个处理在 Router::runRoute(Request $request, Route $route) 方法完成；

- 在运行路由闭包或控制器方法时，将采用类似 HTTP kernel 的 handle 执行方式去运行当前路由适用的局部中间件；
- 在最终的 then 方法内部会执行 $route->run() 方法运行路由，$route（Illuminate\Routing\Route） 为 findRoute 方法查找到的路由；

3、生成 HTTP 响应（由 prepareResponse 方法完成）。

### 执行路由闭包或控制器

```
src/Illuminate/Routing/Route.php

		/**
     * Run the route action and return the response.
     *
     * @return mixed
     */
    public function run()
    {
        $this->container = $this->container ?: new Container;

        try {
            if ($this->isControllerAction()) {
                return $this->runController();
            }

            return $this->runCallable();
        } catch (HttpResponseException $e) {
            return $e->getResponse();
        }
    }

/**
    * Checks whether the route's action is a controller.
    *
    * @return bool
    */
    protected function isControllerAction()
    {
       return is_string($this->action['uses']) && ! $this->isSerializedClosure();
    }

    /**
     * Run the route action and return the response.
     *
     * @return mixed
     */
    protected function runCallable()
    {
        $callable = $this->action['uses'];

        if ($this->isSerializedClosure()) {
            $callable = unserialize($this->action['uses'])->getClosure();
        }

        return $callable(...array_values($this->resolveMethodDependencies(
            $this->parametersWithoutNulls(), new ReflectionFunction($callable)
        )));
    }

    /**
     * Run the route action and return the response.
     *
     * @return mixed
     *
     * @throws \\\\Symfony\\\\Component\\\\HttpKernel\\\\Exception\\\\NotFoundHttpException
     */
    protected function runController()
    {
        return $this->controllerDispatcher()->dispatch(
            $this, $this->getController(), $this->getControllerMethod()
        );
    }

    /**
     * Get the controller instance for the route.
     *
     * @return mixed
     */
    public function getController()
    {
        if (! $this->controller) {
            $class = $this->parseControllerCallback()[0];

            $this->controller = $this->container->make(ltrim($class, '\\\\\\\\'));
        }

        return $this->controller;
    }

    /**
     * Get the controller method used for the route.
     *
     * @return string
     */
    protected function getControllerMethod()
    {
        return $this->parseControllerCallback()[1];
    }

    /**
     * Get the dispatcher for the route's controller.
     *
     * @return \\\\Illuminate\\\\Routing\\\\Contracts\\\\ControllerDispatcher
     */
    public function controllerDispatcher()
    {
        if ($this->container->bound(ControllerDispatcherContract::class)) {
            return $this->container->make(ControllerDispatcherContract::class);
        }

        return new ControllerDispatcher($this->container);
    }
```

### 运行控制器方法

```
src/Illuminate/Routing/ControllerDispatcher.php

    /**
     * Dispatch a request to a given controller and method.
     *
     * @param  \\\\Illuminate\\\\Routing\\\\Route  $route
     * @param  mixed  $controller
     * @param  string  $method
     * @return mixed
     */
    public function dispatch(Route $route, $controller, $method)
    {
        $parameters = $this->resolveClassMethodDependencies(
            $route->parametersWithoutNulls(), $controller, $method
        );

        if (method_exists($controller, 'callAction')) {
            return $controller->callAction($method, $parameters);
        }

        return $controller->{$method}(...array_values($parameters));
    }
```

问题举例：

1、/user/info，怎么匹配路由

2、/user/:userId/info， 怎么匹配路由





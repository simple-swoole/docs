# 控制器

通过控制器来处理对应的`HTTP`请求，需要通过配置文件将路由与控制器方法进行绑定。

## 编写控制器

```php
<?php

declare(strict_types=1);

namespace App\Controller;

class IndexController
{
    public function index($request, $response)
    {
        $response->end("hello swoole");
    }

    public function test($request, $response, $data)
    {
        $response->end(json_encode($data));
    }
}
```

## 中间件

在对应控制器中添加一个`$middleware`属性，设置对应的回调方法

* 类中间件

```php
class IndexController
{
    public $middleware = ['__construct' => [__CLASS__ . '::test']];

    public static function test($handler)
    {
        return function ($request, $response, $vars) use ($handler) {
            // do something
            return $handler($request, $response, $vars);
        };
    }

    public function index($request, $response)
    {
        $response->end(
            json_encode(
                [
                    'method' => $request->server['request_method'],
                    'message' => 'Hello Simps.'
                ]
            )
        );
    }
}
```

* 方法中间件

```php
class IndexController
{
    public $middleware = ['index' => [__CLASS__ . '::test']];

    public static function test($handler)
    {
        return function ($request, $response, $vars) use ($handler) {
            // do something
            return $handler($request, $response, $vars);
        };
    }

    public function index($request, $response)
    {
        $response->end(
            json_encode(
                [
                    'method' => $request->server['request_method'],
                    'message' => 'Hello Simps.'
                ]
            )
        );
    }
}
```
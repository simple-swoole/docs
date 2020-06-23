# 路由

HTTP服务器的路由使用 [nikic/fast-route](https://github.com/nikic/FastRoute) 进行解析，在监听`onRequest`事件中进行路由解析分发到对应的控制器。

## 路由定义

在`config/routes.php`文件中声明对应的路由信息

```php
return [
    ['GET', '/', '\App\Controller\IndexController@index'],
    ['POST', '/', '\App\Controller\IndexController@index'],
    ['GET', '/hello[/{name}]', '\App\Controller\IndexController@hello'],
    ['GET', '/favicon.ico', function ($request, $response) {
        $response->end('');
    }],
];
```

## 路由参数

对应的控制器方法默认有两个参数`$request`和`$reponse`，如果增加了路由参数的话，则在第三个参数数组之中。

```php
public function hello($request, $response, $data)
{
    $name = $data['name'] ?? 'Simps';

    $response->end(
        json_encode(
            [
                'method' => $request->server['request_method'],
                'message' => "Hello {$name}.",
            ]
        )
    );
}
```

!> 关于Request和Response对象完整的介绍请查看Swoole文档：[Http\Request](https://wiki.swoole.com/#/http_server?id=httprequest) 、 [Http\Response](https://wiki.swoole.com/#/http_server?id=httpresponse)

例如默认的路由`/hello[/{name}]`，这个时候访问对应的`http://0.0.0.0:9501/hello/swoole`时，就会输出

```json
{
    "method": "GET",
    "message": "Hello swoole."
}
```
